						## Tìm hiểu về Gem Delayed_job
### 1. Khái niệm:
  - Delayed_job là một asynchronously background processing. Được sử dụng để xử lý các tác vụ có thời gian thực thi lâu hoặc các tác vụ sẽ được chạy trong tương lai. ví dụ các tác vụ có tể là: gửi thư, resize ảnh, dowload file, import file,..
  - Sau khi cài đặt xong delayed_job thì trong csdl sẽ tạo ra 1 bảng với tên gọi là delayed_job. Các tác vụ chạy ngầm sẽ được lưu vào bảng này và sau đó delayed_job sẽ xử lý từng tác vụ. Mỗi tác vụ sau khi chạy hoàn thành thì record của tác vụ đó sẽ bị xóa khỏi bảng này. Nếu tác vụ chạy không thành công, nó sẽ được thực hiện lại đến khi hết lần thử tối đa, nếu vẫn không thành công thì sẽ bị xóa vào 1 thời gian sau đó.
### 2. Cách tạo:
  - Có 3 cách để tạo delayed_job: 
	+ delay: chỉ cần gọi .delay.method(params) với bất kì object nào cần xử lý ở background. Các đối tượng sẽ được đưa vào bảng delayed_job một cách tuần tự.
	+ handle_asynchronously: Được sử dụng khi một phương thức luôn được chạy background, và được khai báo sau khi đinh nghĩa phương thức.
	+ enqueue: Có thể tạo ra một job tùy chỉnh. Dùng Delayed::Job.enqueue để yêu cầu Delayed Job đẩy các tác vụ vào hàng đợi. 
  - Để chạy Delayed_Job: thêm gem "daemons" vào gemfile để chạy, hoặc dùng lệnh rake jobs:work
### 3. Cơ chế hoạt động:
  - Sau khi các job được thêm vào hàng đợi để run được job cần chạy : RAILS_ENV=production bin/delayed_job start ( hoặc các thông số thiết lập khác với từng mục đích cụ thể), lệnh này sẽ start worker ( được hiểu là process), worker access vào database và đồng bộ với thời gian, mỗi 1 worker sẽ kiểm tra database 5 giây 1 lần ( 5s do SLEEP_DELAY thiếp lập, có thể điều chỉnh được thời gian này). Worker sẽ check và chạy các job khi đến thời điểm.
  - Các job nếu fail sẽ chạy lại với số lần attemp đã thiết lập, tối đa là 25 lần, sau 25 lần job có thể bị xóa khởi db hoặc vẫn lưu lại với thời gian failed_at
  - Có thể thiết lập nhiều worker cho 1 job hoặc mỗi job một worker
  - Có thể auto start Delayed::job với gem thứ 3, mặc định delayed::job không cho tự động chạy


