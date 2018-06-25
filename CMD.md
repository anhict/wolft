<pre>timedatectl set-timezone Asia/Ho_Chi_Minh
. admin-openrc
openstack network create  --share --external --provider-physical-network provider --provider-network-type flat provider

openstack subnet create --network provider \
  --allocation-pool start=192.168.239.230,end=192.168.239.240 \
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
  
  
  
  
  
  
  
  </pre>
