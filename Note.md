``` sh
git config --global user.name "anhbka"
git config --global user.email "anhnt@mail.com"
cat ~/.gitconfig
config --list
```



<pre>timedatectl set-timezone Asia/Ho_Chi_Minh
. admin-openrc
openstack network create  --share --external --provider-physical-network provider --provider-network-type flat provider

openstack subnet create --network provider \
  --allocation-pool start=192.168.239.210,end=192.168.239.250 \
  --dns-nameserver 8.8.8.8 --gateway 192.168.239.2 \
  --subnet-range 192.168.239.0/24 provider

  
openstack subnet create --network selfservice \
  --dns-nameserver 8.8.8.8 --gateway 10.10.10.1 \
  --subnet-range 10.10.10.0/24 selfservice  


ovs-vsctl set bridge br0 protocols=OpenFlow13
ovs-vsctl set-fail-mode br0 secure
ovs-vsctl list-br
ovs-vsctl list-ports br0
ovs-vsctl list interface
ovs-ofctl show  // hien thi thong tin ngan gon ve port
ovs-dpctl show //show port
ovs-dpctl dump-flows  

chmod +x *.sh
  
neutron -netlist
openstack show flavor
nova boot --flavor m1.nano --image 0d628fbc-3169-4a12-9361-664119a06b7c  --nic net-id=5f54b578-01a2-4d9f-acf0-faaf926f832e  --availability-zone nova:compute1 vm03  
  
 
openstack security group rule create --proto icmp default
openstack security group rule create --proto tcp --dst-port 22 default
  
openstack flavor create --id 0 --vcpus 1 --ram 64 --disk 1 m1.nano

. admin-openrc
wget http://download.cirros-cloud.net/0.3.5/cirros-0.3.5-x86_64-disk.img
apt update && apt dist-upgrade -y

superuser ALL=(ALL) NOPASSWD:ALL
%supergroup  ALL=(ALL) NOPASSWD:ALL



</pre>
<pre>Cần nắm dược 5 – 4 – 3 trong Cloud Computing: đó là 5 đặc tính, 4 mô hình dịch vụ và 3 mô hình triển khai.

5 đặc điểm: (giải thích chi tiết sẽ có bài viết cụ thể hơn)

Khả năng thu hồi và cấp phát tài nguyên (Rapid elasticity)
Truy nhập qua các chuẩn mạng (Broad network access)
Dịch vụ sử dụng đo đếm được (Measured service,) hay là chi trả theo mức độ sử dụng pay as you go.
Khả năng tự phục vụ (On-demand self-service).
Chia sẻ tài nguyên (Resource pooling).
4 mô hình dịch vụ (mô hình sản phẩm): cũng sẽ giải thích chi tiết hơn trong bài viết cụ thể

Public Cloud: Đám mây công cộng (là các dịch vụ trên nền tảng Cloud Computing để cho các cá nhân và tổ chức thuê, họ dùng chung tài nguyên).
Private Cloud: Đám mây riêng (dùng trong một doanh nghiệp và không chia sẻ với người dùng ngoài doanh nghiệp đó)
Community Cloud: Đám mây cộng đồng (là các dịch vụ trên nền tảng Cloud computing do các công ty cùng hợp tác xây dựng và cung cấp các dịch vụ cho cộng đồng. Tôi cũng chưa rõ FB có phải là một dạng này không, cần xác nhận lại.
Hybrid Cloud : Là mô hình kết hợp (lai) giữa các mô hình Public Cloud và Private Cloud (không rõ có Community Cloud nữa không … :D)
3 mô hình triển khai: tức là triển khai Cloud Computing để cung cấp:

Hạ tầng như một dịch vụ (Infrastructure as a Service)
Nền tảng như một dịch vụ (Platform as a Service)
Phần mềm như một dịch vụ (Software as a Service)
## 10. Launch máy ảo

**Provider network**

- Tạo provider network

``` sh
. admin-openrc
openstack network create  --share --external --provider-physical-network provider --provider-network-type flat provider
```

- Tạo subnet

``` sh
openstack subnet create --network provider \
  --allocation-pool start=192.168.100.201,end=192.168.100.250 \
  --dns-nameserver 8.8.8.8 --gateway 192.168.100.1 \
  --subnet-range 192.168.100.0/24 provider
```

**Self-service network**

- Tạo self-service network

``` sh
. demo-openrc
openstack network create selfservice
```

- Tạo subnet

``` sh
openstack subnet create --network selfservice \
  --dns-nameserver 8.8.8.8 --gateway 10.10.10.1 \
  --subnet-range 10.10.10.0/24 selfservice
```

- Tạo router

`openstack router create router`

- Thêm self-service vào interface của router

`neutron router-interface-add router selfservice`

- Set gateway của router là provider network để ra ngoài internet

`neutron router-gateway-set router provider`

- Kiểm tra lại

``` sh
ip netns
neutron router-port-list router
```

Tiến hành ping tới gateway của router để kiểm tra.

- Tạo m1.nano flavor

Flavor này chỉ dùng để test với CirrOS image

`openstack flavor create --id 0 --vcpus 1 --ram 64 --disk 1 m1.nano`

- Thêm security group rules cho phép ping và ssh

``` sh
openstack security group rule create --proto icmp default
openstack security group rule create --proto tcp --dst-port 22 default
```

- Bạn có thể truy cập vào dashboard hoặc dùng câu lệnh để tạo máy ảo

``` sh
openstack server create --flavor m1.nano --image cirros \
  --nic net-id=NET_ID --security-group default \
   provider-instance
```

Thay `NET_ID` bằng id của provider hoặc self-service network.
</pre>




``` sh
Mở  đầu 1. Tính cấp thiết của đề tài Đại hội lần thứ VII Đảng Cộng sản Việt  Nam khẳng định lấy chủ nghĩa Mác - Lênin,  tư tưởng  Hồ Chí Minh làm  nền tảng  tư tưởng  kim chỉ nam cho hành động của Đảng và dân tộc. Văn kiện Đại hội đại biểu toàn quốc lần IX của Đảng cộng sản Việt Nam tiếp tục khẳng định: Tư tưởng Hồ Chí Minh là một hệ thống quan điểm toàn diện và sâu sắc về những vấn đề cơ bản của cách mạng Việt Nam, là kết quả của sự vận dụng và phát triển sáng tạo chủ  nghĩa Mác - Lênin  vào  điều kiện cụ thể ở nước  ta, kế thừa và  phát triển các  giá trị truyền thống của dân tộc, tiếp thu tinh  hoa văn hóa nhân loại. Đó là tư tưởng về giải phóng dân tộc, giải phóng giai cấp, giải phóng con người; về độc lập dân tộc gắn liền với chủ nghĩa xã hội, kết hợp sức mạnh dân tộc với sức mạnh thời đại; về sức mạnh của nhân dân, của khối đại đoàn kết dân tộc; về quyền làm chủ của nhân dân, xây dựng Nhà nước thực sự của dân, do dân, vì dân... Tư  tưởng  Hồ  Chí Minh  soi đường  cho cuộc  đấu  tranh của  nhân dân ta giành thắng lợi, là tài sản tinh thần to lớn của Đảng và dân tộc ta. Do vậy, học tập, nghiên cứu tư tưởng Hồ Chí Minh là một trong những nhiệm vụ đặt ra trong giai đoạn hiện nay, trong đó có tư tưởng về Nhà nước kiểu mới, Nhà nước của dân, do dân, vì dân. Trong quá trình xây dựng và phát triển đất nước đã và đang đặt ra nhiều vấn đề trong đó có việc hoàn thiện Nhà nước dân chủ, Nhà nước pháp quyền xã hội chủ nghĩa. Hiện nay, đất nước ta đang trong quá trình vận động của nền kinh tế thị trường theo định hướng xã hội chủ nghĩa đòi hỏi phải có sự quản lý Nhà nước với trình độ tương ứng. Hơn nữa, xu hướng toàn cầu hóa, đa phương hóa quan hệ quốc tế cũng đang đặt ra nhiệm vụ không nhỏ đối với Nhà nước. Hơn nửa thập kỷ qua, Nhà nước Việt Nam do Hồ Chí Minh sáng lập và lãnh đạo dần được củng cố và hoàn thiện, đã đạt được những thành tựu lớn, điều này được các 
 2 văn kiện của Đảng ta đánh giá cao. Tuy nhiên, thực tế hoạt động của Nhà nước hiện nay vẫn còn nhiều bất cập, được thể hiện trên nhiều phương diện. Đội ngũ cán bộ công chức vừa thiếu vừa yếu, đăc biệt là sự hiểu biết và cập nhật pháp luật chưa cao để đáp ứng nhu cầu trong xã hội mới. Bên cạnh tình trạng vi phạm pháp luật, áp dụng không đúng pháp luật diễn ra ở các vùng trong cả nước còn nhiều. Có thể nói đó là sự tác động của nền kinh tế thị trường một phần, phần khác là trong chúng ta chưa có sự quan tâm đúng mức về nó hoặc quan tâm chưa đồng  bộ  đến  việc  xây  dựng,  hoàn  thiện  hệ  thống  hành  lang  pháp  lý  của  Nhà nước.  Sinh thời, Nguyễn ái Quốc rất  ít dùng khái niệm “Nhà  nước kiểu  mới”, nhưng qua hành  động thực tiễn thì đã toát  lên tư tưởng của  mình về  việc  xây dựng Nhà nước kiểu mới, khác với các Nhà nước đã có trước đó trong lịch sử Việt Nam. Thực tế cải cách nền hành chính và bộ máy Nhà nước trong giai đoạn hiện nay là tất yếu. Nghị quyết Đại hội đại biểu toàn quốc lần IX của đảng đã ghi rõ: “Quá trình ấy phải dựa trên cơ sở vận dụng một cách sáng tạo chủ nghĩa Mác - Lênin, tư tưởng Hồ Chí Minh, xuất phát từ thực tiễn đất nước và những kinh nghiệm tiên tiến trên thế giới “Vì những nhu cầu lý luận và thực tiễn nêu trên, chúng tôi chọn  “Tư tưởng Hồ Chí  Minh về Nhà nước kiểu mới và việc vận dụng vào xây dựng Nhà nước xã hội chủ nghĩa hiện nay ở nước ta” làm chủ đề nghiên cứu của luận văn này.

```
