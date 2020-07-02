Cùng với sự phát triển nhanh như vũ bão trong thời gian vừa qua. Các kiến trúc DL ngày càng trở lên thông minh hơn, minh chứng là điểm số SOTA qua hàng năm đều có sự tăng trưởng lớn gần mức đạt tới hiệu xuất con nguời trên một số tác vụ. Đạt đưọc thành quả đó, nhờ một phần lớn nhờ đòn bẩy từ việc phát triển các hệ thống phần cứng ưu việt hơn qua từng năm. TPUv3.8 do Google phát triển với khả năng tính toán hơn 440TF, GPU Nvidia 2080Ti,.. đều là các cỗ máy đáng mơ uớc với dân lập trình AI hiện nay. Tuy nhiên với giá thành quá cao không phù hợp với đại đa số sinh viên , nghiên cứu sinh hiện nay. Cùng đó là khả năng phát triển phần cứng thay đổi hàng năm là một chướng ngại rất lớn đối với những nguời say mê lĩnh vực này. Google như thuờng lệ vẫn là bên support cho chúng ta nhiệt tình nhất với việc cho phép sử dụng google colab, với khả năng code và chạy code online, train model nhanh chóng thuận tiện. Cùng với đó là tính năng cho phép sử dụng TPU,GPU của google thực sự là lựa chọn đáng cân nhắc. Nếu bạn mua gói Colab Pro với giá 10$/1month bạn sẽ thuận tiện hơn khi thao tác với colab.

Google Drive là công cụ luư trữ miễn phí với 15gb dữ liệu. Các dự án code hoàn toàn có thể lưu trữ tại đây.

VScode Server cho phép bạn sử dụng VScode ngay trên trình duyệt với đầy đủ tính năng như bản cài đặt.

Sau đây, tôi sẽ hướng dẫn cách kết hợp 3 công cụ này với nhau. Và có thể là Github nữa cho bạn một môi truờng phát triển phần mềm đầy đủ.

Bước 1: Server ngrokngrok tokenBạn cần server để thực hiện ssh từ máy ảo colab - ở đây tôi sử dụng ngok. Việc tạo tài khoản ngrok và token là hoàn toàn miễn phí.

Bước 2: Setup server ngrok trên colabđể thực hiện ssh từ xa máy ảo của colab. thêm mật khẩu ssh và token ở mục mình đánh dấu sẵn

# Install useful stuff! apt-get install --yes ssh screen nano htop ranger git > /dev/null# SSH setting! echo "root:<password>" | chpasswd! echo "PasswordAuthentication yes" > /etc/ssh/sshd_config! echo "PermitUserEnvironment yes" >> /etc/ssh/sshd_config! echo "PermitRootLogin yes" >> /etc/ssh/sshd_config! service ssh restart > /dev/null# Download ngrok! wget -q -c -nc https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip! unzip -qq -n ngrok-stable-linux-amd64.zip# Run ngrokauthtoken = "<token>"get_ipython().system_raw('./ngrok authtoken $authtoken && ./ngrok tcp 22 &')! sleep 3# Get the address for SSHimport requestsfrom re import subr = requests.get('http://localhost:4040/api/tunnels')str_ssh = r.json()['tunnels'][0]['public_url']str_ssh = sub("tcp://", "", str_ssh)str_ssh = sub(":", " -p ", str_ssh)str_ssh = "ssh root@" + str_sshprint(str_ssh)

Các bạn sẽ nhận được lệnh để thực hiện ssh vào máy ảo colab của mình. Lệnh có dạng như sau.

ssh root@0.tcp.ngrok.io -p 14386

Chỉ cần copy và chạy lệnh trên terminal là bạn đã có thể access vào máy ảo như một remote destop với đầy đủ thư mục như một máy UBUNTU đầy đủ.

Buớc 3: Mount Google Drive vào máy ảo colab.để thực hiện lưu trữ và code trực tuyến

# Mount Google Drive and make some folders for vscodefrom google.colab import drivedrive.mount('/googledrive')! mkdir -p /googledrive/My\ Drive/colabdrive! mkdir -p /googledrive/My\ Drive/colabdrive/root/.local/share/code-server! ln -s /googledrive/My\ Drive/colabdrive /! ln -s /googledrive/My\ Drive/colabdrive/root/.local/share/code-server /root/.local/share/

Bước 4: Tải và cài đặt VScode Server! curl -fsSL https://code-server.dev/install.sh | sh > /dev/null! code-server --bind-addr 127.0.0.1:9999 --auth none &

Cuối cùng tạo một season ssh để thao tác trên máy ảo đã tạo

ssh -L 9999:localhost:9999 root@0.tcp.ngrok.io -p 14386

Mở trình duyệt lên và cùng tận huơng thành quả : http://127.0.0.1:9999

Vscode ServerCác file đã code sẽ đưọc lưu trong google drive. Thao tác trên VSCode Server đem lại trải nghiệm tốt và linh hoạt hơn trên Colab.
Enjoy VSCode on Colab.

Nếu bạn thấy hữu ích vui lòng nhấn cho mình 1 claps
