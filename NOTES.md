# Báo cáo: Đưa code Express lên AWS Lambda

## Chiến lược em chọn: Dùng thư viện `serverless-http`
Dạ, để đưa cái app Node.js Express của team mình lên chạy trên AWS Lambda, em đã quyết định chọn dùng gói `serverless-http` ạ. 

## Tại sao em lại chọn cách này?
1. **Gần như không phải sửa code cũ:** Cách này nhàn nhất ở chỗ em không cần đụng chạm hay sửa một dòng nào trong logic cũ của file `app.js` hay setup ở `server.js` cả. Em chỉ cần tạo thêm đúng một file `lambda.js` cỏn con tầm 3 dòng để làm "cổng chào" cho Lambda là code chạy bon bon luôn ạ.
2. **Quen thuộc và an toàn:** Em tìm hiểu thì thấy đây là pattern phổ biến và "chuẩn bài" nhất để chạy Express trên Lambda rồi. Nó tự động dịch cái mớ event phức tạp của API Gateway thành cặp `req`/`res` quen thuộc của Node.js, nên em không cần đập đi xây lại kiến trúc hay phải setup mấy cái layer phức tạp (như cái AWS Lambda Web Adapter).
3. **Chạy ngon trên máy Windows của em:** Vì giải pháp này 100% là code Javascript thuần, nên em code trên máy Windows không bị dính mấy cái lỗi lằng nhằng kiểu phải cấp quyền thực thi `chmod +x run.sh` (mấy cái đó hay làm máy Windows của em bị lỗi vặt lắm ạ 😭).

## Thời gian "Cold Start" (Khởi động lạnh) em đo được
- **Kết quả đo:** Lúc em test thử gọi API lần đầu (qua lệnh curl) thì thấy mất tầm **~750ms** (đây là em tính cả thời gian trễ của mạng internet từ máy em lên AWS rồi ạ). Còn thời gian bản thân thằng Lambda thực sự "vươn vai thức dậy" trong hệ thống AWS thì em check log thấy chỉ mất cỡ **300-400ms** thôi. Nhìn chung là cũng khá mượt mà cho cái app nhỏ xinh của mình sếp ạ!
