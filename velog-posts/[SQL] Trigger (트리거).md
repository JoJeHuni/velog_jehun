<h1 id="trigger">Trigger</h1>
<p>트리거는 '업무 무결성'때문에 하는 것으로, 하나의 트랜잭션이 2개의 테이블에 영향을 줘야할 때 자동으로 트리거를 </p>
<h2 id="1-주문-예제">1. 주문 예제</h2>
<pre><code class="language-sql">DELIMITER //

CREATE OR REPLACE TRIGGER after_oder_menu_insert
   AFTER insert
   ON tbl_order_menu
   FOR EACH ROW -- &quot;tbl_order_menu 의 각 행마다 insert를 하고 나서&quot; 의 의미
BEGIN
    UPDATE tbl_order
    SET total_order_price = total_order_price + NEW.order_amount * (SELECT menu_price 
                                                                      FROM tbl_menu 
                                                                     WHERE menu_code = NEW.menu_code)
    WHERE order_code = NEW.order_code;
END//

DELIMITER ;</code></pre>
<p>만든 뒤에 확인해보자.</p>
<pre><code class="language-sql">SHOW TRIGGERS;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4cf2c3c1-8b59-45ed-9d5c-ddd54cb56b1a/image.png" /></p>
<hr />
<p>트리거가 만들어진 것이 확인됐으니
주문 테이블(<code>tbl_order</code>에 <code>insert</code> 후 주문 메뉴 테이블 (<code>tbl_order_menu</code>)에 주문한 메뉴마다 <code>insert</code> 후 주문 테이블의 총 금액이 업데이트되는지 확인한다.</p>
<pre><code class="language-sql">INSERT
  INTO tbl_order
(
  order_code, order_date
, order_time, total_order_price
)
VALUES
(
  NULL
, DATE_FORMAT(NOW(), '%Y%m%d')
, DATE_FORMAT(NOW(), '%H%i%s')
, 0
);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a033f4d2-1e50-4aa1-9ba8-116b035b9b31/image.png" /></p>
<p>일단은 order 테이블에 insert를 해주었고 tbl_order_menu에 추가해보면 트리거가 발동될 것이다.</p>
<pre><code class="language-sql">INSERT
  INTO tbl_order_menu
(
  order_code
, menu_code
, order_amount
)
VALUES
(
  1
, 2
, 3
);</code></pre>
<p>menu_code = 2인 메뉴는 우럭스무디로 5000원이며, amount = 3이니까 15000원이 되는 것을 알 수 있다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/092a32e5-e45b-4557-9a6b-a087b499bbc9/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1190f7b3-8108-46fc-a5ba-dcabe3a0ac75/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5071085d-ab5c-40d6-b5e6-97577296abee/image.png" /></p>
<hr />
<h2 id="2-상품-입-출고-예제">2. 상품 입, 출고 예제</h2>
<p>입, 출고가 발생하면 제품 이력에 변화가 생기는 것을 자동화하기 위해 트리거를 사용해본다.</p>
<p>1) 이력 테이블 (update가 발생하는 테이블)</p>
<pre><code class="language-sql">CREATE TABLE product(
  pcode INT PRIMARY KEY AUTO_INCREMENT,
  pname VARCHAR(30),
  brand VARCHAR(30),
  price INT,
  stock INT DEFAULT 0,
  CHECK(stock &gt;= 0)
);</code></pre>
<p>2)내역 테이블 (insert가 발생하는 테이블)</p>
<pre><code class="language-sql">CREATE TABLE pro_detail(
  dcode INT PRIMARY KEY AUTO_INCREMENT,
  pcode INT,
  pdate DATE,
  amount INT,
  STATUS VARCHAR(10) CHECK(STATUS IN ('입고', '출고')),
  FOREIGN KEY (pcode) REFERENCES product(pcode)
);</code></pre>
<p>2개의 테이블을 생성했으니 트리거를 만들어보자.</p>
<pre><code class="language-sql">DELIMITER //

CREATE OR REPLACE TRIGGER trg_product_after
    AFTER INSERT
    ON pro_detail
    FOR EACH ROW
BEGIN
    if NEW.status = '입고' then
      UPDATE product
            SET stock = stock + NEW.amount;
    ELSEIF NEW.status = '출고' then
      UPDATE product
            SET stock = stock - NEW.amount;
    END if;
END //

DELIMITER ;</code></pre>
<blockquote>
<p><code>if</code> 는 Java 와 같이 조건문처럼 사용된다.</p>
</blockquote>
<pre><code class="language-sql">    if NEW.status = '입고' then                   -- pro_detail에서 status가 '입고'면
      UPDATE product                         -- product 를 update를 해준다.
            SET stock = stock + NEW.amount;   -- stock (재고) 를 (현재 재고) + (추가되는 양) 으로 업데이트 해주는 것이다.</code></pre>
<p>-&gt; 출고는 반대로 <code>SET</code> 에서 <code>STOCK = (현재 재고) - (빠져나가는 양)</code> 으로 업데이트해준다.</p>
<p>이제 제품을 아무렇게나 만들어서 insert 해주자.</p>
<pre><code class="language-sql">INSERT
  INTO product
(
  pcode, pname, brand
, price, stock
)
VALUES
(
  NULL, '아무개폰1', '아무개1'
, 2100000, 5
);

INSERT
  INTO product
(
  pcode, pname, brand
, price, stock
)
VALUES
(
  NULL, '아무개폰2', '아무개2'
, 1700000, 5
);

INSERT
  INTO product
(
  pcode, pname, brand
, price, stock
)
VALUES
(
  NULL, '아무개폰3', '아무개3'
, 3000000, 5
);</code></pre>
<p>제품의 재고는 각 5, 5, 5개씩 있다.</p>
<pre><code class="language-sql">INSERT
  INTO pro_detail
(
  dcode, pcode, pdate
, amount, status
)
VALUES
(
  NULL, 3, CURDATE()
, 5, '입고'
);

SELECT * FROM product;

INSERT
  INTO pro_detail
(
  dcode, pcode, pdate
, amount, status
)
VALUES
(
  NULL, 2, CURDATE()
, 3, '출고'
);</code></pre>
<p>이 2가지를 하나씩 실행한 뒤에는 바뀌는 것을 확인할 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4e126628-f180-4225-bfb3-1e83481200ee/image.png" /></p>
<p>이렇게 트리거를 끝낸다.</p>