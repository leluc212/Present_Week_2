---
theme: default
title: Báo cáo quá trình học việc
author: Lê Tấn Lực
info: |
  Báo cáo quá trình học việc: Linux doanh nghiệp, Ảo hóa và Container.
fonts:
  sans: Be Vietnam Pro
  weights: '400,500,600,700,800'
aspectRatio: 16/9
canvasWidth: 1280
colorSchema: light
transition: fade
duration: 15min
exportFilename: slides-export
layout: cover
image: /images/cover-main.jpg
image2: /images/cover-small.jpg
---

# Báo cáo quá trình học việc

<div class="mt-8 mb-8">
  <span class="lpc-pill">Linux doanh nghiệp · Ảo hóa · Container</span>
</div>

Người trình bày:

**Lê Tấn Lực (Intern)**

Tháng 11/2026

<!--
Em chào anh/chị. Hôm nay em xin báo cáo quá trình học việc của em, tập trung vào nền tảng hạ tầng: Linux dòng doanh nghiệp, ảo hóa và container.
-->

---
layout: agenda
---

::title::

<div class="lpc-script">Nội dung</div>

::default::

<div class="agenda-grid">
  <div class="agenda-item">01. Ảo hóa &amp; vòng đời VM</div>
  <div class="agenda-item">05. Service &amp; Firewall</div>
  <div class="agenda-item">02. Network</div>
  <div class="agenda-item">06. Quản lý tài nguyên</div>
  <div class="agenda-item">03. Storage</div>
  <div class="agenda-item">07. Container</div>
  <div class="agenda-item">04. User &amp; Permission</div>
  <div class="agenda-item">08. Tổng kết &amp; Định hướng</div>
</div>

<!--
Bài báo cáo đi theo thứ tự một máy chủ cần có để đi vào hoạt động: có máy, nối mạng, có ổ đĩa, có người dùng và quyền, chạy dịch vụ có firewall bảo vệ, theo dõi tài nguyên, và cuối cùng là container.
-->

---
layout: title-right
image: /images/racks.jpg
---

<span class="lpc-tag">Cách tiếp cận</span>

# Học theo vòng đời máy chủ

<TitleLine width="200px" />

<v-clicks>

<ol class="lpc-steps">
  <li>Có máy: VM trên hypervisor</li>
  <li>Nối mạng: IP, routing, DNS</li>
  <li>Có chỗ lưu dữ liệu: LVM, mount</li>
  <li>Có người dùng và quyền</li>
  <li>Chạy dịch vụ, có firewall bảo vệ</li>
  <li>Theo dõi CPU, RAM, disk</li>
  <li>Đóng gói bằng container</li>
</ol>

</v-clicks>

<p class="!text-lg text-[var(--lpc-muted)] !mt-3">Thực hành trên <b>Rocky Linux</b> (tương thích RHEL) và <b>Docker</b>. Phần hypervisor mới ở mức lý thuyết.</p>

<!--
Thay vì học từng chủ đề rời rạc, em sắp xếp kiến thức theo đúng thứ tự một máy chủ cần có để đi vào hoạt động. Phần trước luôn là tiền đề cho phần sau.
Em thực hành trên các máy ảo Rocky Linux và dùng Docker. Riêng hypervisor và vòng đời VM em mới tìm hiểu lý thuyết.
-->

---
layout: title-left-card
---

<span class="lpc-tag">01 · Ảo hóa (lý thuyết)</span>

# Hypervisor

<TitleLine width="260px" />

- Lớp phần mềm **chia tài nguyên** của một máy vật lý (CPU, RAM, ổ đĩa, card mạng) thành nhiều **VM độc lập**
- CPU hiện đại hỗ trợ sẵn: **Intel VT-x, AMD-V**, nên hiệu năng gần như máy thật
- Ví dụ: 1 máy chủ 32 CPU / 128 GB RAM chạy được **10–15 VM** (web, database, mail…)

::right::

<div class="flex flex-col gap-4 h-full justify-center">
  <div>
    <h2 class="!m-0">Type 1 · bare-metal</h2>
    <p class="!my-1 !text-xl">Chạy thẳng trên phần cứng. Dùng trong datacenter, cloud.</p>
    <p class="!my-0 !text-lg text-[var(--lpc-muted)]">ESXi · Hyper-V · KVM · Proxmox</p>
  </div>
  <div class="h-px bg-[#dbe3f0]" />
  <div>
    <h2 class="!m-0">Type 2 · hosted</h2>
    <p class="!my-1 !text-xl">Chạy như ứng dụng trên OS có sẵn. Hợp để học, thử nghiệm.</p>
    <p class="!my-0 !text-lg text-[var(--lpc-muted)]">VirtualBox · VMware Workstation</p>
  </div>
  <div class="rounded-xl bg-[var(--lpc-soft)] px-4 py-3 text-xl leading-snug">
    Hyper-V trông như một tính năng của Windows, nhưng thực chất là <b class="lpc-ring">Type 1</b>: Windows chạy <i>bên trên</i> hypervisor.
  </div>
</div>

<!--
Hypervisor đứng giữa điều phối để các VM dùng chung phần cứng mà không ảnh hưởng nhau. Ảo hóa giúp tiết kiệm phần cứng, điện, chỗ đặt máy, và tạo máy mới chỉ mất vài phút.
Điểm em thấy thú vị: khi bật Hyper-V, hypervisor được nạp xuống dưới cùng và chính Windows trở thành một phân vùng chạy trên nó. Vì vậy VirtualBox trên cùng máy có thể chậm đi. EC2 của AWS cũng chạy trên hypervisor Nitro dựa trên KVM.
-->

---
layout: title-right
image: /images/laptop-code.jpg
---

<span class="lpc-tag">01 · Ảo hóa (lý thuyết)</span>

# VM & vòng đời VM

<TitleLine width="200px" />

- VM thực chất là **file**: file cấu hình + file ổ đĩa ảo (cấp phát thin hoặc thick)

<div class="lpc-flow">
  <span class="lpc-chip">Tạo</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Cài đặt</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Vận hành</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Bảo trì</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Thu hồi</span>
</div>

- Doanh nghiệp tạo **template** rồi **clone** hàng loạt máy giống nhau
- **Overcommit**: cấp vượt tài nguyên thật. Quá tay thì các VM cùng chậm

<!--
VM gồm một file cấu hình mô tả phần cứng ảo và một hoặc nhiều file ổ đĩa ảo: VDI của VirtualBox, VHDX của Hyper-V, VMDK của VMware, QCOW2 của KVM. Vì VM chỉ là file nên sao chép, di chuyển, sao lưu dễ như dữ liệu.
Vòng đời: tạo, cài đặt và cấu hình (chính là các phần 2 đến 5), vận hành, bảo trì (snapshot, backup, clone), thu hồi.
Hypervisor Type 1 còn có live migration: chuyển VM đang chạy sang máy vật lý khác mà không phải tắt.
-->

---
layout: side-panel
label: Bảo vệ VM
---

::left::

<h2 class="!m-0">Snapshot</h2>
<p class="!text-xl !mt-1 italic text-[var(--lpc-muted)]">Như nút "Undo" trong Word</p>

- Lưu trạng thái VM tại một thời điểm
- Quay lại gần như tức thì
- Nằm ngay cạnh VM: hỏng ổ cứng máy chủ là **mất cả hai**

::default::

# Snapshot ≠ Backup

<TitleLine width="200px" />

<div v-click>

<h2 class="!m-0">Backup</h2>
<p class="!text-xl !mt-1 italic text-[var(--lpc-muted)]">Như bản sao cất ở két sắt nhà khác</p>

- Bản sao **đầy đủ, độc lập**, lưu ở nơi khác
- Khôi phục được kể cả khi mất cả máy chủ

<div class="flex gap-3 mt-4">
  <div class="lpc-mini flex-1 text-center"><h3 class="!text-3xl">3</h3><p>bản dữ liệu</p></div>
  <div class="lpc-mini flex-1 text-center"><h3 class="!text-3xl">2</h3><p>loại thiết bị</p></div>
  <div class="lpc-mini flex-1 text-center"><h3 class="!text-3xl">1</h3><p>bản ở nơi khác</p></div>
</div>

</div>

<!--
Đây là điểm em thấy quan trọng nhất của phần ảo hóa. Sau khi chụp snapshot, ổ đĩa gốc được đóng băng, mọi thay đổi ghi vào file chênh lệch. Quay về chỉ cần bỏ file chênh lệch. Nhưng vì vậy snapshot phụ thuộc ổ gốc.
Ví dụ: trước khi cập nhật OS cho web server thì chụp snapshot. Cập nhật lỗi thì quay lại vài giây. Thành công thì xóa snapshot, vì giữ lâu file chênh lệch phình to, VM chậm dần. Backup vẫn chạy hằng đêm, độc lập.
Nguyên tắc 3-2-1: 3 bản dữ liệu, trên 2 loại thiết bị, 1 bản ở nơi khác.
-->

---
layout: title-left-card
---

<span class="lpc-tag">02 · Network</span>

# Network

<TitleLine width="260px" />

- `192.168.56.10/24`: máy thuộc mạng `192.168.56.0`, tối đa 254 địa chỉ
- Cùng mạng thì gửi thẳng. Khác mạng thì đi qua **gateway** (routing)
- 4 chế độ mạng VM: **NAT · Host-only · Bridged · Internal**

<LessonCard class="mt-5">

Khai báo gateway cho cả hai card mạng → máy có 2 default route → **mất Internet**. Mỗi máy chỉ nên có **một** default route.

</LessonCard>

::right::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)]">Đặt IP tĩnh bằng nmcli</p>

```bash
nmcli con mod enp0s8 \
  ipv4.method manual \
  ipv4.addresses 192.168.56.10/24
nmcli con up enp0s8
```

<p class="!text-lg !mb-0 !mt-3 font-600 text-[var(--lpc-navy)]">Đọc bảng định tuyến</p>

```bash
$ ip route
default via 10.0.2.2 dev enp0s3
192.168.56.0/24 dev enp0s8
```

<p class="!text-base !mt-2 text-[var(--lpc-muted)]">Gói tin ra Internet đi qua card NAT, gói tin tới các VM khác đi thẳng qua enp0s8.</p>

<!--
Mỗi máy cần IP và subnet mask để biết mình thuộc mạng nào. Quyết định gói tin đi đường nào chính là routing.
Chế độ mạng VM em mới tìm hiểu lý thuyết: NAT ra được Internet nhưng bên ngoài không vào được; Host-only thì máy thật và VM thấy nhau nhưng không ra Internet; Bridged giống máy thật trong LAN; Internal chỉ các VM nói chuyện với nhau. Cách bố trí phổ biến: mỗi VM hai card, một NAT để tải phần mềm, một mạng nội bộ để quản trị.
DNS: em cấu hình qua NetworkManager để không bị ghi đè /etc/resolv.conf, và khai báo tên các máy trong /etc/hosts.
-->

---
layout: title-right
image: /images/ethernet.jpg
---

<span class="lpc-tag">02 · Network</span>

# Mất kết nối? Kiểm tra theo lớp

<TitleLine width="200px" />

<v-clicks>

<ol class="lpc-steps">
  <li><span>Card mạng có IP chưa? <code>ip addr</code></span></li>
  <li><span>Có đường đi chưa? <code>ip route</code></span></li>
  <li><span>Ping được gateway không?</span></li>
  <li><span>Ping được IP đích không?</span></li>
  <li><span>Ping bằng tên được không? (DNS)</span></li>
</ol>

</v-clicks>

<p class="!text-xl !mt-4">Kiểm tra từng lớp giúp <b>khoanh vùng lỗi</b> nhanh hơn nhiều so với đoán mò.</p>

<!--
Quan trọng nhất ở phần network là em có được thứ tự kiểm tra khi mất kết nối. Đi từ dưới lên: card mạng, đường đi, gateway, IP đích, rồi mới đến DNS.
Khi mạng đã thông, em chuyển sang chuẩn bị chỗ lưu dữ liệu cho máy chủ.
-->

---
layout: title-right
---

::left::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)] text-center">3 lớp của LVM</p>

<div class="lvm">
  <div class="lvm-row">
    <div class="lvm-box">PV <code>/dev/sdb1</code></div>
    <div class="lvm-box dashed">PV <code>/dev/sdc1</code><br/><small>thêm sau</small></div>
  </div>
  <div class="lvm-arrow">↓</div>
  <div class="lvm-box wide">VG <b>vg_data</b> · "bể chứa"</div>
  <div class="lvm-arrow">↓</div>
  <div class="lvm-box wide">LV <b>lv_web</b> (5G)</div>
  <div class="lvm-arrow">↓</div>
  <div class="lvm-box wide navy">XFS → mount <b>/data</b></div>
</div>

<style>
.lvm { display:flex; flex-direction:column; align-items:center; gap:4px; margin-top:12px; }
.lvm-row { display:flex; gap:14px; }
.lvm-box { background:var(--lpc-soft); border:2px solid var(--lpc-navy); border-radius:12px; padding:10px 16px; font-size:1.15rem; text-align:center; }
.lvm-box.dashed { border-style:dashed; }
.lvm-box.wide { width:340px; }
.lvm-box.navy { background:var(--lpc-navy); color:#fff; }
.lvm-box.navy b { color:#fff; }
.lvm-arrow { color:var(--lpc-orange); font-weight:800; font-size:1.5rem; line-height:1; }
</style>

::default::

<span class="lpc-tag">03 · Storage</span>

# Storage & LVM

<TitleLine width="200px" />

- Không để dữ liệu ứng dụng **chung ổ với OS**
- Đầy chỗ: thêm ổ vào bể, nới LV ngay khi máy chạy: `lvextend -r`
- `/etc/fstab` dùng **UUID**, không dùng `/dev/sdb`

<LessonCard class="mt-4">

Sai một dòng fstab → máy vào emergency mode. Luôn chạy `mount -a` trước khi reboot. XFS chỉ nới được, không thu nhỏ được.

</LessonCard>

<!--
Nếu log hoặc dữ liệu web làm đầy ổ, cả máy chủ có thể ngừng hoạt động. Vì vậy em gắn thêm một ổ ảo 10 GB vào web01 và đi đủ các bước: lsblk, fdisk tạo partition, pvcreate, vgcreate, lvcreate 5G, mkfs.xfs, mount vào /data.
Lợi ích lớn nhất của LVM: hết dung lượng chỉ cần thêm ổ vào bể (vgextend) rồi nới LV (lvextend -r), không phải chia lại ổ.
Tên thiết bị kiểu /dev/sdb có thể đổi khi thêm bớt ổ, nên fstab dùng UUID lấy từ blkid.
Có chỗ chứa dữ liệu rồi, câu hỏi tiếp theo là ai được phép đọc, ghi vào đó.
-->

---
layout: title-left-card
---

<span class="lpc-tag">04 · User & Permission</span>

# User & Permission

<TitleLine width="260px" />

- Mỗi file có **owner · group · other**, quyền **r = 4, w = 2, x = 1**. Ví dụ `750` = `rwx r-x ---`
- **Least privilege**: chỉ cấp đúng quyền cần thiết, ví dụ chỉ cho restart httpd

<LessonCard class="mt-5">

`usermod -G wheel an` (thiếu <b class="lpc-ring">-a</b>) ghi đè danh sách nhóm, An bị xóa khỏi webteam. Phải dùng `usermod -aG`.

</LessonCard>

::right::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)]">Tình huống: An và Bình cùng làm /data/web</p>

```bash
groupadd webteam
useradd -m -G webteam an
useradd -m -G webteam binh
chown root:webteam /data/web
chmod 2770 /data/web
```

```text
drwxrws---. root webteam /data/web
```

<p class="!text-lg !mt-2">Số <b>2</b> là <b>setgid</b> (chữ <b>s</b>): file mới tự thuộc nhóm webteam, nên Bình sửa được file của An.</p>

```bash
# /etc/sudoers.d/webteam
%webteam ALL=(root) \
  /usr/bin/systemctl restart httpd
```

<!--
Linux là hệ điều hành nhiều người dùng, mọi file trả lời ba câu hỏi: ai sở hữu, thuộc nhóm nào, người còn lại được làm gì.
Tình huống: team web gồm An và Bình cùng làm trong /data/web, người ngoài nhóm không được xem. Nếu không có setgid, file của An sẽ thuộc nhóm riêng của An và Bình không sửa được.
Trên RHEL, thành viên nhóm wheel được sudo toàn quyền. Nhưng team web chỉ cần restart dịch vụ web, nên em cấp đúng một lệnh đó qua sudoers.
Lệnh sudo nhắc đến httpd, đó là dịch vụ web em dựng ở phần tiếp theo.
-->

---
layout: title-right
---

::left::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)]">Unit file tự viết</p>

```ini {5,7}
[Unit]
Description=Hello App demo
After=network.target
[Service]
User=an
ExecStart=/usr/bin/python3 -m http.server 8080
Restart=on-failure
[Install]
WantedBy=multi-user.target
```

<p class="!text-lg !mt-2">Chạy bằng user <b>an</b>, không chạy bằng root. Thử <code>kill -9</code> tiến trình: systemd <b>tự chạy lại</b>.</p>

::default::

<span class="lpc-tag">05 · Service & Firewall</span>

# systemd

<TitleLine width="200px" />

- Tiến trình đầu tiên khi khởi động (**PID 1**), quản lý mọi dịch vụ
- `start` = chạy ngay bây giờ · `enable` = tự chạy khi boot
- Làm cả hai: `systemctl enable --now httpd`
- Xem log: `journalctl -u httpd -n 50`

<!--
Em phân biệt hai khái niệm dễ nhầm: start là chạy ngay, enable là cho chạy mỗi khi khởi động. Muốn cả hai thì enable --now.
Để hiểu sâu hơn, em tự viết unit file cho một ứng dụng nhỏ, chạy dưới quyền user An đúng tinh thần least privilege của phần 4. Sau đó em kill -9 tiến trình chính, và systemctl status cho thấy systemd đã tự chạy lại nhờ Restart=on-failure, không cần người can thiệp.
-->

---
layout: side-panel
label: Web không vào được?
---

::left::

<p class="!text-xl !m-0 font-600">Kiểm tra lần lượt từng lớp:</p>

<ol class="lpc-steps !mt-3">
  <li><span>Dịch vụ đã chết? <code>systemctl status</code></span></li>
  <li><span>Firewall chặn? <code>firewall-cmd</code></span></li>
  <li><span>Sai quyền file? <code>ls -ld</code></span></li>
  <li><span>SELinux chặn? (lỗi <b>403</b>)</span></li>
</ol>

::default::

# Firewall & SELinux

<TitleLine width="200px" />

- **firewalld** chia theo zone, mở từng service hoặc port
- Lỗi 403 dù quyền rwx đúng: **SELinux** chặn httpd đọc `/data/web`

```bash
firewall-cmd --permanent --add-service=http
firewall-cmd --reload
semanage fcontext -a -t httpd_sys_content_t "/data/web(/.*)?"
restorecon -Rv /data/web
```

<LessonCard class="mt-3">

Thiếu `--permanent`: rule mất sau reload. Có `--permanent` mà quên `--reload`: rule chưa có hiệu lực.

</LessonCard>

<!--
Dịch vụ chạy rồi nhưng từ Windows vẫn chưa truy cập được, vì firewalld mặc định chặn các cổng chưa mở. Mỗi card mạng thuộc một zone, trong zone ta cho phép service hoặc port cụ thể.
Khi em đổi thư mục web của httpd sang /data/web, trình duyệt báo 403 dù quyền đã đúng. Nguyên nhân là SELinux, lớp bảo mật mặc định của RHEL, chặn httpd đọc thư mục không có nhãn phù hợp. Em mới tìm hiểu SELinux ở mức cơ bản nhưng đã biết gán lại nhãn.
Từ lỗi này em rút ra: web không vào được có thể do 4 nguyên nhân, phải kiểm tra lần lượt, giống cách kiểm tra mạng ở phần 2.
-->

---
layout: title-left-card
---

<span class="lpc-tag">06 · Quản lý tài nguyên</span>

# Quản lý tài nguyên

<TitleLine width="260px" />

Tình huống tự đặt: **"web01 chạy chậm"**. Điều tra theo thứ tự CPU → RAM → disk → tiến trình.

- **CPU**: `top`, chỉ số `wa` cao nghĩa là nghẽn ở **disk**, không phải CPU
- **RAM**: nhìn cột **available**, không nhìn cột free
- **Giới hạn** từng dịch vụ bằng systemd:

```bash
systemctl set-property hello-app.service \
  CPUQuota=50% MemoryMax=256M
```

<p class="!text-lg !mt-1">Cơ chế đứng sau là <b>cgroups</b>, cũng chính là cơ chế Docker dùng.</p>

::right::

<div class="flex flex-col h-full justify-center gap-5">
  <div class="text-center">
    <p class="!text-lg !m-0 text-[var(--lpc-muted)]">load average trên máy <b>2 CPU</b></p>
    <p class="!m-0 font-800 text-[var(--lpc-navy)]" style="font-size:5rem;line-height:1.1">3.95</p>
    <p class="!text-xl !m-0">Việc chờ xử lý gần <b>gấp đôi</b> khả năng → quá tải</p>
  </div>
  <div class="h-px bg-[#dbe3f0]" />
  <div>

```text
$ free -h
      total  used   free   available
Mem:  1.9Gi  620Mi  110Mi  1.1Gi
```

  </div>
  <p class="!text-lg !m-0">Free chỉ 110 MB nhưng không thiếu RAM: Linux dùng RAM trống làm cache. Thiếu thật khi swap tăng liên tục hoặc có "Out of memory".</p>
</div>

<!--
Em tạo tải giả lập: yes > /dev/null để chiếm CPU, dd ghi file lớn để làm đầy disk, rồi tập điều tra.
nproc cho biết máy có 2 CPU, uptime cho load average 3.95, nghĩa là quá tải. Trong top nếu wa cao thì nghẽn ở disk, chi tiết này giúp em không kết luận vội.
Disk và tiến trình: df -h xem phân vùng nào đầy, du -sh tìm thư mục chiếm nhiều nhất, ps aux sort theo CPU. Dừng tiến trình thì kill -15 trước, kill -9 chỉ khi không phản hồi. renice để hạ ưu tiên tiến trình backup.
Phát hiện quan trọng nhất: systemd giới hạn được tài nguyên cho từng dịch vụ nhờ cgroups. Phát hiện này dẫn em sang phần container.
-->

---
layout: title-right
image: /images/port-containers.jpg
---

<span class="lpc-tag">07 · Container</span>

# Container vs VM

<TitleLine width="200px" />

<div class="lpc-mini !mb-3">
  <h3>VM · như nhà riêng</h3>
  <p>Mỗi nhà có móng riêng: <b>kernel và OS riêng</b>. Nặng vài GB, khởi động vài phút.</p>
</div>

<div v-click class="lpc-mini" style="border-top-color: var(--lpc-orange)">
  <h3>Container · như căn hộ chung cư</h3>
  <p>Dùng chung móng: <b>chung kernel</b> của máy chủ. Mỗi căn có cửa khóa riêng nhờ <b>namespaces</b>, giới hạn tài nguyên bằng <b>cgroups</b>. Khởi động vài giây.</p>
</div>

<!--
Sau khi làm việc với VM, em thấy rõ sự khác biệt của container. VM ảo hóa phần cứng, còn container ảo hóa ở mức hệ điều hành.
Namespaces giúp mỗi container thấy tiến trình, mạng, file riêng. cgroups là cơ chế em đã gặp ở phần 6.
Em cài Docker trên web01. Docker cũng chỉ là một dịch vụ systemd. Lưu ý thành viên nhóm docker có quyền gần như root, nên chỉ thêm người thật sự cần.
-->

---
layout: agenda
---

::title::

<h1 class="!text-5xl">Docker trong thực hành</h1>

::default::

<div class="grid grid-cols-2 gap-4">
  <div class="lpc-mini">
    <h3>Image & Registry</h3>
    <p>Dockerfile → <code>docker build</code> → push lên <b>registry nội bộ</b>. Gắn tag <b>1.0</b> thay vì latest.</p>
  </div>
  <div class="lpc-mini">
    <h3>Volume</h3>
    <p>Xóa container PostgreSQL, tạo lại cùng volume: <b>dữ liệu vẫn còn</b>. Bind mount trên RHEL cần <code>:Z</code>.</p>
  </div>
  <div class="lpc-mini">
    <h3>Network</h3>
    <p>User-defined network có <b>DNS nội bộ</b>: container web gọi database bằng tên <code>db</code>.</p>
  </div>
  <div class="lpc-mini">
    <h3>Resource limit</h3>
    <p><code>--memory 256m --cpus 0.5</code>. Vượt RAM thì kernel kill container (<b>OOMKilled</b>).</p>
  </div>
</div>

<LessonCard class="mt-4">

Khi publish port (`-p`), Docker tự thêm rule firewall riêng. Port có thể mở ra ngoài dù chưa khai báo trong firewalld, nên chỉ publish port thật sự cần.

</LessonCard>

<!--
Image là bản đóng gói chỉ đọc, xây theo layer, Docker cache các layer không đổi nên build lần sau rất nhanh. Em đóng gói trang web tĩnh từ nginx:1.27-alpine.
Em tự dựng registry nội bộ: web01 đẩy image lên, db01 kéo về. Registry trong lab chạy HTTP nên phải khai báo insecure-registries, môi trường thật cần HTTPS.
Volume: container có thể xóa và tạo lại bất cứ lúc nào, dữ liệu cần giữ lâu như database phải đặt ra volume. Hậu tố :Z để Docker gán nhãn SELinux, đây lại là lỗi 403 em từng gặp ở phần 5.
Network: bridge mặc định không phân giải tên container, user-defined network thì có. Đây là ý tưởng DNS ở phần 2 ở quy mô nhỏ.
Resource limit: giống cgroups với systemd ở phần 6, theo dõi bằng docker stats.
-->

---
layout: title-right
---

::left::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)] text-center">Lab xuyên suốt: web hai tầng trên 2 VM</p>

<div class="lab">
  <div class="lab-node">Máy Windows</div>
  <div class="lab-arrow">↓ <small>HTTP</small></div>
  <div class="lab-vm">
    <div class="lab-title">web01 · 192.168.56.10</div>
    <div class="lab-item">httpd · firewalld · SELinux</div>
    <div class="lab-item">LVM → /data · nhóm webteam</div>
    <div class="lab-item">Docker: web + db (volume) + registry</div>
  </div>
  <div class="lab-arrow">↓ <small>push / pull image</small></div>
  <div class="lab-vm">
    <div class="lab-title">db01 · 192.168.56.11</div>
    <div class="lab-item">docker pull từ registry nội bộ</div>
  </div>
</div>

<style>
.lab { display:flex; flex-direction:column; align-items:center; gap:4px; margin-top:10px; }
.lab-node { background:var(--lpc-soft); border:2px solid var(--lpc-navy); border-radius:10px; padding:6px 18px; font-size:1.1rem; }
.lab-vm { width:430px; border:2px solid var(--lpc-navy); border-radius:14px; padding:8px 12px; background:#fff; }
.lab-title { font-weight:700; color:var(--lpc-navy); font-size:1.15rem; margin-bottom:4px; }
.lab-item { background:var(--lpc-soft); border-radius:8px; padding:4px 10px; margin:4px 0; font-size:1.05rem; }
.lab-arrow { color:var(--lpc-orange); font-weight:800; font-size:1.3rem; line-height:1.1; }
.lab-arrow small { color:var(--lpc-muted); font-weight:500; font-size:0.95rem; }
</style>

::default::

<span class="lpc-tag">08 · Tổng kết</span>

# 4 bài học

<TitleLine width="200px" />

<v-clicks>

<ol class="lpc-steps">
  <li><span><b>Kiến thức không tách rời.</b> "Không vào được web" có thể do mạng, firewall, quyền, SELinux hay dịch vụ chết</span></li>
  <li><span><b>Kiểm tra từng lớp</b>, từ dưới lên, thay vì đoán mò</span></li>
  <li><span><b>Đọc log trước khi sửa</b>: journalctl, systemctl status, dmesg</span></li>
  <li><span><b>Luôn có đường lui</b>: snapshot hoặc backup trước mọi thay đổi lớn</span></li>
</ol>

</v-clicks>

<!--
Để kiểm tra mình có thật sự hiểu không, em làm lại toàn bộ thành một bài lab xuyên suốt: hệ thống web hai tầng trên hai VM web01 và db01 chạy Rocky Linux.
IP tĩnh bằng nmcli, tên trong /etc/hosts. Trên web01 gắn ổ, tạo LVM, mount /data, tạo nhóm webteam, phân quyền 2770. httpd bật bằng systemd, firewalld chỉ mở HTTP và SSH, SELinux gán đúng nhãn. Giả lập tải để luyện theo dõi. Cuối cùng chuyển ứng dụng sang container: image ở registry nội bộ, database dùng volume, container nói chuyện qua user-defined network, mỗi container có giới hạn RAM, CPU.
Qua quá trình này em rút ra 4 điều.
-->

---
layout: title-left-card
---

<span class="lpc-tag">08 · Định hướng</span>

# Tự đánh giá

<TitleLine width="260px" />

<div class="lpc-mini !mb-3">
  <h3>Đã nắm</h3>
  <p>Thao tác cơ bản, và hiểu <b>"vì sao"</b> phía sau từng thao tác.</p>
</div>

<div class="lpc-mini" style="border-top-color: var(--lpc-orange)">
  <h3>Còn ở mức làm quen / lý thuyết</h3>
  <p>Vòng đời VM (snapshot, backup, template) · SELinux · chia subnet phức tạp · xử lý sự cố hiệu năng thực tế · script tự động hóa</p>
</div>

::right::

<p class="!text-xl !m-0 font-700 text-[var(--lpc-navy)]">Lộ trình sắp tới</p>

<ol class="lpc-steps !mt-3">
  <li><span><b>Củng cố nền tảng</b> (liên tục): thực hành VM, làm lại lab không xem hướng dẫn, viết script Bash</span></li>
  <li><span><b>Kubernetes</b>: dựng cụm kubeadm trên 3 VM Rocky Linux</span></li>
  <li><span><b>AWS SAA-C03</b>: lab trên Free Tier, có cảnh báo chi phí</span></li>
  <li><span><b>Red Hat RHCSA</b> (song song), sau đó <b>RHCE</b> với Ansible</span></li>
</ol>

<!--
Em tự đánh giá thẳng thắn: em đã nắm thao tác cơ bản và hiểu lý do phía sau. Nhưng còn nhiều phần mới ở mức làm quen. Đây cũng là lý do lộ trình bắt đầu bằng việc củng cố.
Giai đoạn 1: tự tạo VM bằng dòng lệnh trên VirtualBox và Hyper-V, snapshot trước mỗi thay đổi lớn, export VM để backup và thử khôi phục, tạo template rồi clone. Viết script Bash: tạo user hàng loạt, backup định kỳ bằng systemd timer, kiểm tra tình trạng máy chủ. Ghi chép lỗi đã gặp thành tài liệu nội bộ.
Giai đoạn 2: Kubernetes điều phối hàng trăm container trên nhiều máy. Cụm gồm 1 control plane, 2 worker.
Giai đoạn 3: SAA-C03 gồm 4 nhóm: bảo mật 30%, khả năng phục hồi 26%, hiệu năng 24%, tối ưu chi phí 20%.
Giai đoạn 4: RHCSA (EX200) thi thực hành trên RHEL 10, bài làm phải còn đúng sau khi reboot. Em sẽ đào sâu SELinux và học Podman. Sau đó là RHCE (EX294) về Ansible.
-->

---
layout: agenda
---

::title::

<h1 class="!text-5xl">Từ nền tảng đến K8s & AWS</h1>

::default::

| Kiến thức đã học | Kubernetes | AWS |
|---|---|---|
| VM | – | EC2 |
| Network, routing, DNS | Service | VPC, subnet, route table |
| Firewall | – | Security Group |
| Storage, volume | PersistentVolume | EBS, EFS, S3 |
| User, group, least privilege | – | IAM |
| Snapshot, backup | – | EBS snapshot, AWS Backup |
| Registry, container | Cụm điều phối container | ECR, ECS, EKS |
| cgroups (giới hạn tài nguyên) | Requests & limits | – |
| systemd | kubelet trên mỗi node | – |
| Theo dõi tài nguyên | – | CloudWatch |

<!--
Lý do em chọn thứ tự này: hầu hết khái niệm của Kubernetes và AWS đều nối với những gì em vừa học. Vì vậy em không phải học từ con số không, mà chỉ cần học cách các khái niệm quen thuộc vận hành trên cụm và trên cloud.
-->

---
layout: qa
---

# Q&A

<!--
Em rất mong nhận được góp ý của anh/chị về lộ trình này, đặc biệt là những kỹ năng team đang cần, để em ưu tiên thứ tự học cho phù hợp.
-->

---
layout: thank-you
image: /images/thanks-main.jpg
image2: /images/thanks-small.jpg
---

<span class="lpc-pill !text-2xl !px-10 !py-4">Cảm ơn anh/chị đã lắng nghe</span>

<TitleLine width="100%" />

<p class="!text-xl text-[var(--lpc-muted)]">Rất mong nhận được góp ý về lộ trình học của em.</p>

<!--
Em cảm ơn anh/chị đã dành thời gian lắng nghe.
-->
