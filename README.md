# GIỚI THIỆU TỔNG QUAN VỀ DỮ LIỆU

## 1. Mô tả dataset

Bộ dữ liệu được lấy từ Cơ quan quản lý doanh nghiệp nhỏ chính phủ Hoa Kỳ (SBA).

  SBA Hoa Kỳ được thành lập năm 1953 trên nguyên tắc thúc đẩy và hỗ trợ các doanh nghiệp nhỏ trên thị trường tín dụng Hoa Kỳ (Tổng quan và Lịch sử SBA, Cơ quan Quản lý Doanh nghiệp Nhỏ Hoa Kỳ (2015)). Các doanh nghiệp nhỏ là nguồn tạo việc làm chính ở Hoa Kỳ; do đó, thúc đẩy sự hình thành và tăng trưởng của doanh nghiệp nhỏ mang lại lợi ích xã hội bằng cách tạo cơ hội việc làm và giảm tỷ lệ thất nghiệp.
  
  Đã có nhiều câu chuyện thành công về việc các công ty khởi nghiệp nhận được bảo lãnh khoản vay SBA như FedEx và Apple Computer. Tuy nhiên, cũng có những câu chuyện về các doanh nghiệp nhỏ và/hoặc công ty mới thành lập đã không trả được các khoản vay do SBA bảo lãnh.
  
  Bộ dữ liệu gồm có 899164 dòng và 27 thuộc tính (tính đến ngày 01/04/2023).
  
  Đường liên kết bộ dữ liệu: [Should This Loan be Approved or Denied?](https://www.kaggle.com/datasets/mirbektoktogaraev/should-this-loan-be-approved-or-denied)

## 2. Danh sách thuộc tính được phân tích

| STT | Thuộc tính | Kiểu dữ liệu | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: |
| 1 | LoanNr_ChkDgt | varchar(20) | Mã khoản vay |
| 2	| Borrower	| nvarchar(200)	| Tên doanh nghiệp vay tiền |
| 3	| City	| nvarchar(50)	| Tên thành phố |
| 4	| State	| nvarchar(10)	| Tên tiểu bang của doanh nghiệp |
| 5	| Bank	| nvarchar(100)	| Tên ngân hàng |
| 6	| BankState	| nvarchar(10)	| Tên tiểu bang của ngân hàng |
| 7	| NAICS	| varchar(10)	| Mã hệ thống phân loại công nghiệp Bắc Mỹ |
| 8	| ApprovalDate	| date	| Ngày cam kết SBA được ban hành |
| 9	| Term	| int	| Thời hạn vay (tính theo tháng) |
| 10	| NoEmp	| int	| Số lượng nhân viên của doanh nghiệp |
| 11	| NewExist	| varchar(1)	| Phân loại doanh nghiệp |
| 12	| UrbanRural	| varchar(1)	| Phân loại khu vực |
| 13	| ChgOffDate	| date	| Ngày mà một khoản vay được tuyên bố là không có khả năng thanh toán |
| 14	| DisbursementDate	| date	| Ngày giải ngân |
| 15	| DisbursementGross	| money	| Số tiền đã giải ngân |
| 16	| MIS_Status	| nvarchar(10)	| Trạng thái của khoản vay |
| 17	| ChgOffPrinGr	| money	| Số tiền khoanh nợ |
| 18	| GrAppv	| money	| Tổng số tiền cho vay đã được ngân hàng phê duyệt |
| 19	| SBA_Appv	| money	| Số tiền được SBA bảo đảm cho khoản vay |

## 3. Xây dựng kho dữ liệu
> Sơ đồ bông tuyết

![](https://github.com/lychthac/DW-OLAP_SBA-guaranteed-loans/issues/1#issue-2445356573)

* BẢNG Dim_State

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| StateID	| int	| Khóa chính	| NOT	| Mã tiểu bang
| 2	| State	| nvarchar(10)	| 	| ALLOW	| Tên tiểu bang

* BẢNG Dim_City

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| CityID	| int	| Khóa chính	| NOT	| Mã thành phố
| 2	| City	| nvarchar(50)	| 	| ALLOW	| Tên thành phố
| 3	| StateID	| int	| Khóa ngoại	| NOT	| Mã tiểu bang

* BẢNG Dim_Borrower

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| BorrowerID	| int	| Khóa chính	| NOT	| Mã số doanh nghiệp vay 
| 2	| Borrower	| nvarchar(200)	| 	| ALLOW	| Tên doanh nghiệp vay 
| 3	| CityID	| int	| Khóa ngoại	| NOT	| Mã thành phố của doanh nghiệp

* BẢNG Dim_Bank

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| BankID	| int	| Khóa chính	| NOT	| Mã số ngân hàng
| 2	| Bank	| nvarchar(100)	| 	| ALLOW	| Tên ngân hàng
| 3	| StateID	| int	| Khóa ngoại	| NOT	| Mã tiểu bang của ngân hàng

* BẢNG Dim_ NAICS

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| NAICSID	| int	| Khóa chính	| NOT	| Mã dùng để phân loại các ngành công nghiệp Bắc Mỹ 
| 2	| NAICS	| varchar(10)	| 	| ALLOW	| Tên mã hệ thống phân loại công nghiệp Bắc Mỹ 

* BẢNG Dim_ Date

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| Date	| date	| Khóa chính	| NOT	| Ngày tháng năm
| 2	| Day	| int	| 	| NOT	| Ngày
| 3	| Month	| int	| 	| NOT	| Tháng
| 4	| Year	| int	| 	| NOT	| Năm

* BẢNG Dim_ ApprovalDate

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| ApprovalDateID	| int	| Khóa chính	| NOT	| Mã ngày cam kết SBA được ban hành
| 2	| ApprovalDate	| date	| Khóa ngoại	| NOT	| Ngày cam kết SBA được ban hành

* BẢNG Dim_ NewExist

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| NewExistID	| int	| Khóa chính	| NOT	| Mã dùng để phân loại doanh nghiệp 
| 2	| NewExist	| varchar(1)	| 	| NOT	| Tên phân loại doanh nghiệp 

* BẢNG Dim_ UrbanRural

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| UrbanRuralID	| int	| Khóa chính	| NOT	| Mã tiểu bang
| 2	| UrbanRural	| varchar(1)	| 	| NOT	| Mã dùng để phân loại khu vực

* BẢNG Dim_ ChgOffDate

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| ChgOffDateID	| int	| Khóa chính	| NOT	| Mã ngày mà một khoản vay được tuyên bố là không có khả năng thanh toán 
| 2	| ChgOffDate	| date	| 	| ALLOW	| Ngày mà một khoản vay được tuyên bố là không có khả năng thanh toán 

* BẢNG Dim_ DisbursementDate

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| DisbursementDateID	| int	| Khóa chính	| NOT	| Mã ngày giải ngân
| 2	| DisbursementDate	| date	| 	| NOT	| Ngày giải ngân

* BẢNG Dim_MIS_Status

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| MIS_StatusID	| int	| Khóa chính	| NOT	| Mã trạng thái của khoản vay
| 2	| MIS_Status	| nvarchar(10)	| 	| NOT	| Tên trạng thái của khoản vay

* BẢNG Fact

| STT | Thuộc tính | Kiểu dữ liệu | Ràng buộc | NULL | Ý nghĩa |
| :-------------------: | :--------------------: | :------------------: | :------------------: | :---------------------: | :---------------------: |
| 1	| LoanNr_ChkDgt	| varchar(20)	| Khóa chính	| NOT| 	Mã khoản vay
| 2	| BorrowerID	| int	| Khóa ngoại	| NOT	| Mã doanh nghiệp
| 3	| BankID	| int	| Khóa ngoại | NOT	| Mã ngân hàng cho vay
| 4	| NAICS	| varchar(10)	| 	| ALLOW	| Mã hệ thống phân loại công nghiệp Bắc Mỹ
| 5	| ApprovalDateID	| int	| Khóa ngoại	| NOT	| Mã ngày cam kết SBA được ban hành
| 6	| Term	| int| 	Khóa ngoại	| NOT	| Mã thời hạn vay (tính theo tháng)
| 7	| NoEmp	| int	| 	| NOT	| Số lượng nhân viên của doanh nghiệp
| 8	| NewExistID	| int	| Khóa ngoại	| NOT	| Mã dùng để phân loại doanh nghiệp
| 9	| UrbanRuralID	| int	| Khóa ngoại	| NOT	| Mã dùng để phân loại khu vực
| 10	| ChgOffDateID	| int	| Khóa ngoại	| NOT	| Mã ngày mà một khoản vay được tuyên bố là không có khả năng thanh toán
| 11	| DisbursementDateID	| int	| Khóa ngoại	| NOT	| Ngày giải ngân
| 12	| DisbursementGross	| money	| 	| NOT	| Số tiền đã giải ngân
| 13	| MIS_StatusID	| int	| Khóa ngoại	| NOT	| Trạng thái của khoản vay
| 14	| ChgOffPrinGr	| money	| 	| NOT	| Số tiền khoanh nợ
| 15	| GrAppv	| money	| 	| NOT	| Tổng số tiền cho vay đã được ngân hàng phê duyệt
| 16	| SBA_Appv	| money| 		| NOT	| Số tiền được SBA bảo đảm cho khoản vay


