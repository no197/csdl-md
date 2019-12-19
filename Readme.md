# Nội dung

### 1. Viết các câu định nghĩa dữ liệu (Data Definition Language - DDL

#### Tạo bảng

```sql



create  table TABLE_NAME(
<TEN_COT>  <KIEU_DU_LIEU> [not null] [primary key],
...

[constraint <TEN_KHOA_CHINH> primary key <CAC_COT>]
[constraint <TEN_KHOA_MGOAI> foreign key (TEN_COT) references <TEN_BANG_REF>(<COT_LA_KHOA_CHINH>)],
...

)



```

- Ví dụ

* Một cột làm khóa chính

```sql
create  table CONGTY(
MACTY char(3) not  null  primary  key,
TENCTY varchar(25),
LOAICTY varchar(15)
)
```

- Nhiều cột làm khóa chính

```sql



create  table VE(
MAHK char(3) not  null,
MACB char(3) not  null,
LOAIVE varchar(10),
NGMUA smalldatetime
constraint PK_VE primary  key (MAHK, MACB)
constraint FK_VE_HK foreign key (MAHK) references HANHKHACH(MAHK),
constraint FK_VE_CB foreign key (MACB) references CHUYENBAY(MACB)
)

```

##### **Lưu ý:**

- Khóa chính not null

- Trường hợp một cột làm khóa ngoại và nhiều cột làm khóa ngoại

#### Sửa cấu trúc bảng

- Thêm thuộc tính vào bảng

```sql

ALTER  TABLE tênbảng ADD têncột kiểudữliệu

```

```sql

ALTER  TABLE KHACHHANG ADD GHI_CHU varchar(20)

```

- Sửa kiểu dữ liệu của một thuộc tính (không phải lúc nào cũng sửa được)

```sql

ALTER  TABLE tênbảng ALTER COLUMN têncột kiểudữliệu_mới

```

- Xóa thuộc tính

```sql

ALTER  TABLE tên_bảng DROP COLUMN tên_cột

```

- Tạo Ràng buộc

![Ràng buộc](https://i.imgur.com/OxNJ3YD.jpg)

- Xóa ràng buộc

```sql

ALTER  TABLE tên_bảng DROP  CONSTRAINT tên_ràng_buộc



```

##### Lưu ý

- Tắt các ràng buộc

```sql

-- Disable all table constraints
ALTER  TABLE MyTable NOCHECK CONSTRAINT ALL

-- Enable all table constraints
ALTER  TABLE MyTable WITH  CHECK  CHECK  CONSTRAINT ALL

-- Disable single constraint
ALTER  TABLE MyTable NOCHECK CONSTRAINT MyConstraint



-- Enable single constraint
ALTER  TABLE MyTable WITH  CHECK  CHECK  CONSTRAINT MyConstraint

```

### 2. Các loại ràng buộc trên bảng

- Ràng buộc trên một dòng ở một bảng

  - Default: [Link (Nên dùng cách 3)](https://v1study.com/sql-rang-buoc-default-a16.html)
  - Unique: [Link](https://v1study.com/sql-rang-buoc-unique-a14.html)
  - Check: [Link](https://v1study.com/sql-rang-buoc-check-a15.html)

- Ràng buộc nhiều dòng trên một bảng hoặc là ở nhiều bảng

  - Trigger
  - Xác định bảng tầm ảnh hưởng
  - Trên những bảng nào
  - Trên những cột nào của bảng ( xem tất cả các cột trên bảng để tránh sót)
  - Viết Trigger (BTH 5)

```sql
--- Đề thi năm 2017 - 2018
---c1.
    GO
create trigger trig_01_i_u ON TAIKHOAN
for insert, update
as
begin
----------Code here

---1 Lấy NgayMo, MaKH từ tài khoản mới thêm vào
Declare @NgayMo smalldatetime, @MaKH char(3)

SELECT @NgayMo = NgayMo, @MaKH = MaKH
from inserted

---2 Lên bảng khách hàng, lấy ra ngày sinh khách hàng tương ứng
Declare @NgaySinh smalldatetime

SELECT @NgaySinh = NgaySinh
from KHACHHANG
where @MaKH = MaKH

---3 Kiểm tra Year(NgayMo) - Year(NgaySinh) >=14
if(Year(@NgayMo) - Year(@NgaySinh) >=14)
begin
  print 'Thao tac thanh cong'
end
else
begin
  print 'Co loi xay ra'
  ROLLBACK TRAN
end
end
```

---

### 3. Truy vấn SQL

1. Truy vấn đơn giản

2. Truy vấn với JOIN
   ![Các kiểu join](http://vietnam12h.com/hoangnam/hinhanh/anhchitietbaiviet/5-phep-chon-trong-sql-4-9-2013.png)
3. Truy vấn với SUM, AVG, COUNT, MIN, MAX
4. Truy vấn với IN, NOT IN

- Ví dụ: Cho biết những khách hàng (MaKH, HoTen, CMND) đã mở cả hai loại tài khoản: tiết kiệm (TenLTK= ‘Tiết kiệm’) và thanh toán (TenLTK= ‘Thanh toán’).

5. Truy vấn lồng

6. Phép chia
