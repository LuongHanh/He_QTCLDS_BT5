# BÀI TẬP 05 - LƯỜNG VĂN HẠNH - K225480106013 - HỆ QUẢN TRỊ CƠ SỞ DỮ LIỆU
## DEADLINE 23:59:59 NGÀY 23/04/2025
## SUBJECT: Trigger on mssql
# ĐỀ BÀI
## A. Trình bày lại đầu bài của đồ án PT&TKHT

1. Mô tả bài toán của đồ án PT&TKHT, đưa ra yêu cầu của bài toán đó
2. Cơ sở dữ liệu của Đồ án PT&TKHT :
   - Có database với các bảng dữ liệu cần thiết (3nf),
   - Các bảng này đã có PK, FK, CK cần thiết
 
## B. Nội dung bài tập 

1. Dựa trên cơ sở là csdl của Đồ án
2. Tìm cách bổ xung thêm 1 (hoặc vài) trường phi chuẩn
   (là trường tính toán đc, nhưng thêm vào thì ok hơn,
    ok hơn theo 1 logic nào đó, vd ok hơn về speed)
   => Nêu rõ logic này!
3. Viết trigger cho 1 bảng nào đó, 
   mà có sử dụng trường phi chuẩn này,
   nhằm đạt được 1 vài mục tiêu nào đó.
   => Nêu rõ các mục tiêu 
4. Nhập dữ liệu có kiểm soát, 
   nhằm để test sự hiệu quả của việc trigger auto run.
5. Kết luận về Trigger đã giúp gì cho đồ án của em.

# HƯỚNG DẪN CÁCH LÀM
## Hướng dẫn làm phần A
 - Chỉ cần nêu ra y/c của đồ án.
 - Không cần chụp quá trình làm ra db, tables.
 - Chỉ cần đưa ra db gồm các bảng nào,
   mỗi bảng có các trường nào, kiểu dữ liệu nào,
   và pk, fk, ck của các bảng.

## Hướng dẫn làm phần B
1. Sv tạo repo mới trên github, cho phép truy cập public.
2. Tạo file Readme.md, đầu file để thông tin cá nhân sv.
3. Tiếp theo đưa phần A vào file Reame.md .
3. Các thao tác làm trên csdl bằng phần mềm ssms.
4. Chụp ảnh màn hình quá trình làm.
5. Paste ngay vào Readme.md, rồi gõ mô tả ảnh này làm gì, nhập gì, hay đạt được điều gì...
6. Có thể thêm những nhận xét hoặc kết luận cho việc bản thân đã hiểu rõ thêm về 1 vấn đề gì đó.
7. Lặp lại các step 4 5 6 cho đến khi hoàn thành yêu cầu của phần B.
8. Xuất các file sql chứa cấu trúc và data, up lên cùng repo.
9. Link đến repo cần mở được trực tiếp nội dung, Paste link này vào file excel online ghim trên nhóm.

# BÀI LÀM

## A. Trình bày lại đầu bài của đồ án PT&TKHT
### 1. Mô tả bài toán của đồ án PT&TKHT, đưa ra yêu cầu của bài toán đó
- Tên đề tài: Hệ thống điều hướng khu vực nội bộ
- Bất cập: Tôi thấy google map chỉ chỉ đường đi ở các khu vực lớn như đường quốc lộ, đường làng,...
Trong khi đó các khu vực như trường học, bệnh viện, các cơ quan,...thì sẽ khó để xem chỉ đường đi. Mọi người hiện nay thường hỏi đường để đi, đi 1 đoạn lại hỏi. Tôi cho rằng như thế rât mất thời gian của nhiều người.
- Ý tưởng: Nhằm khắc phục bất cập đó và chỉ đường chi tiết đến từng phòng, từng địa điểm trong khu vực.
- Yêu cầu: Đảm bảo hệ thống chỉ đường chính xác từ địa điểm này đến điểm nào đó, có tính toán độ dài, thời gian trên từng đoạn đường và cả hành trình.
  
### 2. Cơ sở dữ liệu của Đồ án PT&TKHT 
**Có database với các bảng dữ liệu cần thiết (3nf). Các bảng này đã có PK, FK, CK cần thiết**

*Thiết kế database:*
Tạo db có tên: DHKV_DB.

- Khuvuc(#IdKV, TenKV, LoaiKV, DiaChi, DienTich, ToaDoX, ToaDoY...)
- CongTrinh(#IdCT, TenCT, LoaiCT, SoTang, HinhAnh, ToaDoX(Toạ độ trung tâm của từng công trình), ToaDoY, @IdKV,...)
- DiaDiem(#IdDD, TenDD, TenKhac, LoaiDD, ToaDoX, ToaDoY, TangSo, @IdCT, ...)
- DistaceDD(#IdPathDD, @IdDiemA, @IdDiemB, LoaiDuong(Cầu thang, hành lang,..), DoDai)
- DistaceCT(#IdPathCT, @IdCTA, @IdCTB, DoDai, TinhTrang(Cấm hoặc không cấm),...)
- LogPath(#IdLog, @IdNSD, ThuTu, TgBatDau, TgKetThuc, @IdDDBatDau, @IdDDKetThuc, PathJson)
- ChitietLogPath(#IdCTLP, @IdLog, ThuTu, @IdPathDD, @IdPathCT, TrangThai(Hoàn thành,chưa hoàn thành,..))
- NguoiSuDung(#IdNSD, TenNSD, TenDN, MK, Vaitro(admin, user), DoiTuong(Làm việc học tập trong khu vực, hoặc không) )
- LogPassword(#IdLpass, @IdNSD , OldMK, NewMK, TimeUpdated (getdate()), PathJson)
Chú ý: # là PK, @ là FK. ToaDo là toạ độ trung tâm.

*Các bảng trong database*

![image](https://github.com/user-attachments/assets/a8195be0-889a-414d-a5b1-213cbdc56528)

*Các dữ liệu của database*

![Screenshot (2)](https://github.com/user-attachments/assets/58638a07-ef47-4ffd-9e91-15a305959114)

![Screenshot (3)](https://github.com/user-attachments/assets/e8a627ac-423e-47f7-9bac-6a880597db52)

![Screenshot (4)](https://github.com/user-attachments/assets/6157d45d-242e-4e8e-b708-5142b65d0a98)

![Screenshot (5)](https://github.com/user-attachments/assets/9e2c916d-5ae7-4610-80e9-4ddce57e2b2f)

![Screenshot (6)](https://github.com/user-attachments/assets/240585b7-3271-4c92-9d69-ffde151693be)

![Screenshot (7)](https://github.com/user-attachments/assets/a185555a-0ab8-4857-b9c7-0bd12755423e)

![Screenshot (8)](https://github.com/user-attachments/assets/4a90dc12-2719-42e0-83c1-8c3d836b38cf)

![Screenshot (9)](https://github.com/user-attachments/assets/abcad18f-67ee-44ad-8fd3-4dcd7e35e71e)

![Screenshot (10)](https://github.com/user-attachments/assets/fb110ac0-86ea-4a97-866f-6c897e6851d9)

## B. Nội dung bài tập
### 1. Dựa trên cơ sở là csdl của đồ án
**Code SQL tạo db**
```sql
-- Tạo CSDL
CREATE DATABASE DHKV_DB;

-- Bảng Khuvuc
CREATE TABLE Khuvuc (
    IdKV INT PRIMARY KEY IDENTITY(1,1),
    TenKV NVARCHAR(255) NOT NULL,
    LoaiKV NVARCHAR(100),
    DiaChi NVARCHAR(255),
    DienTich FLOAT,
    ToaDoX FLOAT NOT NULL,
    ToaDoY FLOAT NOT NULL,
    CONSTRAINT CK_Khuvuc_DienTich CHECK (DienTich > 0)
);

-- Bảng CongTrinh
CREATE TABLE CongTrinh (
    IdCT INT PRIMARY KEY IDENTITY(1,1),
    TenCT NVARCHAR(255) NOT NULL,
    LoaiCT NVARCHAR(100),
    SoTang INT,
    HinhAnh NVARCHAR(MAX),
    ToaDoX FLOAT NOT NULL,
    ToaDoY FLOAT NOT NULL,
    IdKV INT NOT NULL,
    FOREIGN KEY (IdKV) REFERENCES Khuvuc(IdKV),
    CONSTRAINT CK_CongTrinh_SoTang CHECK (SoTang >= 0)
);

CREATE TABLE DistanceCT(
    IdPathCT INT PRIMARY KEY IDENTITY(1,1),
    IdCTA INT NOT NULL,
    IdCTB INT NOT NULL,
    DoDai FLOAT NOT NULL,
    TinhTrang NVARCHAR(30),
    FOREIGN KEY (IdCTA) REFERENCES CongTrinh(IdCT),
    FOREIGN KEY (IdCTB) REFERENCES CongTrinh(IdCT),
    CONSTRAINT CK_DistanceCT_DoDai_Positive CHECK (DoDai > 0)
);

-- Bảng DiaDiem
CREATE TABLE DiaDiem (
    IdDD INT PRIMARY KEY IDENTITY(1,1),
    TenDD NVARCHAR(255) NOT NULL,
    TenKhac NVARCHAR(255),
    LoaiDD NVARCHAR(100),
    ToaDoX FLOAT NOT NULL,
    ToaDoY FLOAT NOT NULL,
    TangSo INT,
    IdCT INT NOT NULL,
    FOREIGN KEY (IdCT) REFERENCES CongTrinh(IdCT),
    CONSTRAINT CK_DiaDiem_TangSo CHECK (TangSo >= 0)
);


CREATE TABLE DistanceDD(
    IdPathDD INT PRIMARY KEY IDENTITY(1,1),
    IdDiemA INT NOT NULL,
    IdDiemB INT NOT NULL,
    Loai NVARCHAR(100),
    DoDai FLOAT,
    TinhTrang NVARCHAR(100),
    FOREIGN KEY (IdDiemA) REFERENCES DiaDiem(IdDD),
    FOREIGN KEY (IdDiemB) REFERENCES DiaDiem(IdDD),
    CONSTRAINT CK_DistanceDD_DoDai_Positive CHECK (DoDai > 0)
);

-- Bảng NguoiSuDung
CREATE TABLE NguoiSuDung (
    IdNSD INT PRIMARY KEY IDENTITY(1,1),
    TenNSD NVARCHAR(255) NOT NULL,
    TenDN NVARCHAR(100) NOT NULL,
    MK NVARCHAR(100) NOT NULL,
    VaiTro NVARCHAR(50),
    DoiTuong NVARCHAR(100)
);

-- Bảng LogPath
CREATE TABLE LogPath (
    IdLog INT PRIMARY KEY IDENTITY(1,1),
    IdNSD INT NOT NULL,
    ThuTu INT NOT NULL,
    TgBatDau DATETIME,
    TgKetThuc DATETIME,
    IdDDBatDau INT NOT NULL,
    IdDDKetThuc INT NOT NULL,
    PathJson NVARCHAR(MAX),
    FOREIGN KEY (IdNSD) REFERENCES NguoiSuDung(IdNSD),
    FOREIGN KEY (IdDDBatDau) REFERENCES DiaDiem(IdDD),
    FOREIGN KEY (IdDDKetThuc) REFERENCES DiaDiem(IdDD),
    CONSTRAINT CK_LogPath_ThuTu CHECK (ThuTu > 0),
    CONSTRAINT CK_LogPath_TgBatDau CHECK (TgBatDau > 0),
    CONSTRAINT CK_LogPath_TgKetThuc CHECK (TgKetThuc > 0)
);

-- Bảng ChiTietLogPath
CREATE TABLE ChiTietLogPath (
    IdCTLP INT PRIMARY KEY IDENTITY(1,1),
    IdLog INT NOT NULL,
    ThuTu INT,
    IdPathDD INT NOT NULL,
	IdPathCT INT NOT NULL,
    TrangThai NVARCHAR(20),
    FOREIGN KEY (IdLog) REFERENCES LogPath(IdLog),
    FOREIGN KEY (IdPathDD) REFERENCES DistanceDD(IdPathDD),
	FOREIGN KEY (IdPathCT) REFERENCES DistanceCT(IdPathCT),
    CONSTRAINT CK_ChiTietLogPath_ThuTu CHECK (ThuTu > 0)
);

CREATE TABLE LogPassword (
    IdLpass INT PRIMARY KEY IDENTITY(1,1), 
    IdNSD INT NOT NULL,          
    OldMK NVARCHAR(100) NOT NULL,        
    NewMK NVARCHAR(100) NOT NULL,
    TimeUpdated DATETIME NOT NULL DEFAULT GETDATE(),   
    PathJson NVARCHAR(MAX),                 
    FOREIGN KEY (IdNSD) REFERENCES NguoiSuDung(IdNSD)
);
```
### 3. Sử dụng các trường phi chuẩn 

Trong db của đồ án này, em có bổ sung thêm các trường phi chuẩn như ToaDoX,ToaDoY trong bảng DiaDiem, trường DoDai trong bảng DistanceDD và DistanceCT. Mục tiêu là để tăng speed khi người dùng tìm đường. Hệ thống sẽ có sẵn toạ độ và độ dài từng đoạn đường mà không phải tính lại, chỉ cần tính thời gian hoàn thành ước tính. Điều này sẽ xử lý nhanh đáng kể.
### 4. Các trigger được sử dụng:
```sql 
-- KIỂM TRA TRÙNG LẶP KHU VỰC
-- Tên trigger: CheckDuplicate_KhuVuc
-- Thuộc bảng: Khuvuc
-- Mục đích: Đảm bảo khu vực chưa tồn tại trước khi thêm mới.

-- KIỂM TRA TRÙNG LẶP CÔNG TRÌNH TRONG CÙNG MỘT KHU VỰC
-- Tên trigger: CheckDuplicate_CongTrinh
-- Thuộc bảng: CongTrinh
-- Mục đích: Tránh thêm công trình đã có trong cùng khu vực.

-- KIỂM TRA TRÙNG LẶP ĐỊA ĐIỂM TRONG CÙNG MỘT CÔNG TRÌNH
-- Tên trigger: CheckDuplicate_DiaDiem
-- Thuộc bảng: DiaDiem
-- Mục đích: Đảm bảo mỗi địa điểm trong công trình là duy nhất.

-- KIỂM TRA TRÙNG LẶP TÊN NGƯỜI DÙNG VÀ TÍNH HỢP LỆ CỦA MẬT KHẨU
-- Tên trigger: Check_InsertNSD
-- Thuộc bảng: NguoiSuDung
-- Mục đích: Ngăn chặn việc đăng ký tên người dùng đã tồn tại.

-- KIỂM TRA HỢP LỆ KHI CẬP NHẬT MẬT KHẨU
-- Tên trigger: CheckUpdate_password
-- Thuộc bảng: NguoiSuDung
-- Mục đích: Xác minh việc cập nhật mật khẩu có đủ điều kiện hợp lệ.

-- KIỂM TRA MẬT KHẨU MỚI CÓ TRÙNG VỚI MẬT KHẨU HIỆN TẠI KHÔNG
-- Tên trigger: CheckDuplicate_oldnewMK
-- Thuộc bảng: LogPassword
-- Mục đích: Đảm bảo mật khẩu mới không trùng với mật khẩu cũ.
```
### 5. Nhập dữ liệu có kiểm soát, nhằm để test sự hiệu quả của việc trigger auto run.
**Kết luận: Các trigger hoạt động tốt, đúng mục đích**
**KIỂM TRA TRÙNG LẶP KHU VỰC**
![Screenshot (11)](https://github.com/user-attachments/assets/d2cd6c18-01a8-489d-a3a9-314ee9fdf0a9)
<img width="960" alt="Screenshot (17)" src="https://github.com/user-attachments/assets/66209224-aa96-4295-ac3f-daf897473ab9" />

**KIỂM TRA TRÙNG LẶP CÔNG TRÌNH TRONG CÙNG MỘT KHU VỰC**
![Screenshot (12)](https://github.com/user-attachments/assets/7a7044a5-10fd-40c2-949f-eb4695cb6433)
<img width="960" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/0ab65d0b-e68d-4b59-82c7-bbc3555adfb3" />

**KIỂM TRA TRÙNG LẶP ĐỊA ĐIỂM TRONG CÙNG MỘT CÔNG TRÌNH**
![Screenshot (13)](https://github.com/user-attachments/assets/3b2a9319-6ee7-4a2a-a085-e4e05b017f23)
<img width="960" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/f4be43fc-5972-4dea-964f-d9daca491821" />

**KIỂM TRA TRÙNG LẶP TÊN NGƯỜI DÙNG**
![Screenshot (14)](https://github.com/user-attachments/assets/a4dddcec-6a34-4c5e-9081-3178886cf0bb)
<img width="960" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/950e030f-27a9-4b84-baf1-0a030f67e49b" />

**KIỂM TRA HỢP LỆ KHI CẬP NHẬT MẬT KHẨU**
![Screenshot (15)](https://github.com/user-attachments/assets/a5cef8b4-2f2a-4345-86d8-0ead66894fb0)

**KIỂM TRA MẬT KHẨU MỚI CÓ TRÙNG VỚI MẬT KHẨU HIỆN TẠI KHÔNG**
![Screenshot (16)](https://github.com/user-attachments/assets/5711dc43-0ed5-4444-b05f-746a0318cae2)
<img width="960" alt="Screenshot (22)" src="https://github.com/user-attachments/assets/d8174ba1-6f71-4b16-b443-f65ef628c3b2" />

### 6. Kết luận về Trigger đã giúp gì cho đồ án của em.
Trigger giúp db của bài toán được tốt hơn, các ràng buộc rõ ràng hơn, việc kiểm soát lỗi của hệ thống cũng dễ dàng hơn.
