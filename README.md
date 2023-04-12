***
**<div align = "center">NHÓM 15 PHÁT TRIỂN PHẦN MỀM HƯỚNG DỊCH VỤ</div>**  
Thành viên trong nhóm:
* Nguyễn Văn Đạt - B17DCCN116  
* Hoàng Công Thiện - B18DCCN637
* Ngô Minh Quang - B17DCCN507  
  
***

**I. Đề tài bài tập lớn**  
Xây dựng ứng dụng đặt vé máy bay  
***
**II. Mô tả về đề tài**  
Ứng dụng đặt vé máy bay được xây dựng với mục đích:
* Tạo sự tiện lợi và linh hoạt trong việc đặt vé máy bay đối với các hành khách  
* Tăng khả năng tiếp cận với khách hàng và nâng cao năng suất làm việc đối với các hãng hàng không  
  
Một dịch vụ được thực hiện trong ứng dụng:
- Dịch vụ đặt vé: cho phép người dùng đặt vé trên máy bay và xác nhận thông tin chuyến bay.
***

**III. Phân tích và mô hình hoá với Web services và Microservices**  
Dưới đây là quy trình chi tiết các bước để lập mô hình ứng viên Web Services cho Dịch vụ đặt vé máy bay  

**Bước 1: Phân rã quy trình kinh doanh (Decompose Bussiness Process)**  
Sau đây là chi tiết các bước của quy trình kinh doanh dịch vụ:  
1. Nhận thông tin khách hàng đặt vé:  
* 1a. Nhận thông tin khách hàng và vé khách hàng chọn  
* 1b. Nhập thông tin khách hàng và vé khách hàng chọn  
2. Xác nhận trạng thái vé  
* Kiểm tra thông tin của khách hàng  
* Kiểm tra trạng thái của vé khách hàng đã đặt  
* Kiểm tra thông tin chi tiết của vé  
3. Nếu thông tin đã được xác minh  
* Cập nhật hồ sơ của khách hàng  
* Chấp nhận việc nhập thông tin và xuất thông tin vé cho khách hàng  
* Kết thúc tiến trình  
4. Nếu thông tin không được xác minh, từ chối việc nhập thông tin  
*	Đưa ra thông báo từ chối xuất vé cho khách hàng  

Bảng phân tích chi tiết các bước (workflow) của quy trình kinh doanh:  
*	Nhận thông tin của khách hàng (Thông tin của khách hàng)  
*	Xác nhận trạng thái đặt vé  
*	Nếu thông tin của khách hàng không đầy đủ hoặc chính xác, từ chối việc nhập thông tin  
*	Tạo thông báo giải thích lý do từ chối đặt vé cho khách hàng  
*	Nếu thông tin của khách hàng đã chính xác, cập nhật hồ sơ của khách hàng  
*	Chấp nhận việc nhận thông tin của khách hàng và xuất thông tin vé máy bay cho khách hàng  
*	Kết thúc quy trình  

![Img 01](https://drive.google.com/drive/folders/1prCMSiZaA2bCw74WmTD7Jj5iY6IKDUYS) Hình 1.1. Quy trình khách hàng đặt vé máy bay  


**Bước 2: Lọc ra các hành động không phù hợp (Filter Out Unsuitable Actions)**  
~~1. Nhận thông tin tiêu dùng của khách hàng~~  


**Bước 3: Xác định ứng viên dịch vụ thực thể (Define Entity Service Candidates)**  
![Img 02](https://drive.google.com/drive/folders/1prCMSiZaA2bCw74WmTD7Jj5iY6IKDUYS) Hình 3.1. Mô hình thực thể, thể hiện các thực thể kinh doanh của tiến trình dịch vụ khách hàng đặt vé  
Dưới đây là các ứng viên dịch vụ thực thể và thuộc tính của chúng:  
Khách hàng: Lấy thông tin (Tên, địa chỉ, sđt, ngày sinh)  
Vé máy bay: Xuất thông tin cho khách hàng (Thời gian, địa điểm, vị trí ghế, giá vé )  
![Img 03](https://drive.google.com/drive/folders/1prCMSiZaA2bCw74WmTD7Jj5iY6IKDUYS) Hình 3.2 Ứng cử viên dịch vụ Khách hàng  
![Img 04](https://drive.google.com/drive/folders/1prCMSiZaA2bCw74WmTD7Jj5iY6IKDUYS) Hình 3.3 Ứng cử viên dịch vụ Vé máy bay  
Các ứng viên dịch vụ được coi là không bất khả tri được in đậm (là các ứng viên dịch vụ không thể tái sử dụng và chỉ được dùng cho 1 mục đích duy nhất):  
*	Nhận thông tin của khách hàng  
*	**Xác nhận thông tin của khách hàng**  
*	**Nếu thông tin chưa được đầy đủ hoặc chính xác, từ chối việc nhập thông tin**  
*	Tạo thông báo giải thích lý do từ chối  
*	Đưa ra thông báo từ chối bảng hoá đơn cho nhân viên  
*	**Nếu thông tin của hành khách đã chính xác, cập nhật và lưu hồ sơ của khách hàng**  
*	**Chấp nhận việc nhận thông tin và kết thúc quy trình**


**Bước 4: Xác định logic cụ thể của quy trình (Identify Process Specific Logic)**  
Hành động nhận thông tin và các hành động ở dưới đại diện cho quá trình nhận thông tin của khách hàng:   
**NHẬN THÔNG TIN CỦA KHÁCH HÀNG**  
*	Nhận thông tin của khách hàng đưa ra  
*	Nếu thông tin chưa được đầy đủ hoặc chính xác, từ chối việc xác nhận  
*	Nếu thông tin đầy đủ và chính xác, xác nhận vé cho khách hàng  
*	Chấp nhận việc nhận thông tin và kết thúc quy trình  



