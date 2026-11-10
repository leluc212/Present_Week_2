---
theme: default
title: Báo cáo quá trình học việc
author: Lê Tấn Lực
info: |
  Báo cáo quá trình học việc: Nền tảng hạ tầng - Linux doanh nghiệp, Ảo hóa và Container.
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
  <span class="lpc-pill">Nền tảng hạ tầng: Linux doanh nghiệp, Ảo hóa và Container</span>
</div>

Người trình bày:

**Lê Tấn Lực (Intern)**

Tháng 11/2026

<!--
Em chào anh Khoa và các bạn. Hôm nay em xin báo cáo quá trình học việc của em về nền tảng hạ tầng: Linux doanh nghiệp, ảo hóa và container.
-->

---
layout: agenda
---

::title::

<div class="lpc-script">Nội dung</div>

::default::

<div class="agenda-grid" style="row-gap:22px">
  <div class="agenda-item">Mở đầu</div>
  <div class="agenda-item">5. Service (systemd) và firewall</div>
  <div class="agenda-item">1. Hypervisor và vòng đời VM</div>
  <div class="agenda-item">6. Quản lý tài nguyên máy chủ</div>
  <div class="agenda-item">2. Network</div>
  <div class="agenda-item">7. Container</div>
  <div class="agenda-item">3. Storage</div>
  <div class="agenda-item">Tổng kết</div>
  <div class="agenda-item">4. User, group và permission</div>
  <div class="agenda-item">Định hướng sắp tới</div>
</div>

<!--
Báo cáo đi theo thứ tự mà một máy chủ cần có để đi vào hoạt động, từ máy ảo, mạng, ổ đĩa, người dùng và quyền, dịch vụ và firewall, theo dõi tài nguyên, đến container. Cuối cùng là phần tổng kết và định hướng.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">Nền tảng hạ tầng: Linux doanh nghiệp, Ảo hóa và Container</span>

# Mở đầu

<TitleLine width="260px" />

- Tìm hiểu 3 mảng: **Linux doanh nghiệp, ảo hóa, container**
- Đã cài **RHEL 10.2** và **Docker** để làm quen. Thực hành còn ít, nên báo cáo **tập trung lý thuyết**
- Sắp xếp theo thứ tự một máy chủ cần có, phần trước là tiền đề cho phần sau:

<v-clicks>

<div class="lpc-flow">
  <span class="lpc-chip">Máy ảo</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Mạng</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Ổ đĩa</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">User &amp; quyền</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Dịch vụ &amp; firewall</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Tài nguyên</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Container</span>
</div>

</v-clicks>

- Mỗi phần: khái niệm · cách hoạt động · điểm riêng của RHEL 10 · ví dụ minh họa

<LessonCard class="mt-2">

Các câu lệnh chỉ là **ví dụ minh họa** cho lý thuyết, không phải bài lab đã làm hoàn chỉnh.

</LessonCard>

::right::

<h2 class="!m-0">Vì sao chọn RHEL</h2>

- Nhiều doanh nghiệp sử dụng, là nền tảng của các chứng chỉ Red Hat
- **RHEL 10** ra mắt 5/2025. **10.2** là bản minor, phát hành 5/2026
- Bản minor khoảng 6 tháng một lần, các bản trong dòng 10 tương thích nhau
- Phải đăng ký **subscription** mới truy cập được kho phần mềm:

<div class="flex gap-2 mt-1">
  <div class="lpc-mini flex-1"><h3>BaseOS</h3><p>Phần lõi hệ điều hành</p></div>
  <div class="lpc-mini flex-1"><h3>AppStream</h3><p>Ứng dụng, ngôn ngữ lập trình, công cụ</p></div>
</div>

- **Developer Subscription for Individuals**: miễn phí, tối đa **16 hệ thống**

<!--
Trong thời gian học việc, em tập trung tìm hiểu kiến thức nền tảng về hạ tầng hệ thống gồm ba mảng: Linux dòng doanh nghiệp, ảo hóa và container. Em có cài RHEL 10.2 và Docker để làm quen. Tuy nhiên thời gian thực hành thực tế còn ít, nên báo cáo tập trung trình bày phần lý thuyết em đã hiểu. Các câu lệnh đi kèm chỉ là ví dụ minh họa.
Em chọn RHEL vì nhiều doanh nghiệp sử dụng và là nền tảng của các chứng chỉ Red Hat em định hướng. Khác với các bản Linux cộng đồng, máy RHEL phải đăng ký subscription thì mới truy cập được kho phần mềm.
Thay vì học từng chủ đề rời rạc, em sắp xếp kiến thức theo thứ tự một máy chủ cần có để đi vào hoạt động.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">1. Hypervisor và vòng đời VM</span>

# Hypervisor là gì

<TitleLine width="260px" />

- Lớp phần mềm **chia tài nguyên** máy vật lý (CPU, RAM, ổ đĩa, card mạng) thành nhiều **máy ảo độc lập**
- Mỗi VM có vCPU, RAM, ổ đĩa ảo, card mạng ảo. Hypervisor đứng giữa điều phối để các VM không ảnh hưởng nhau
- CPU hỗ trợ ảo hóa **Intel VT-x, AMD-V**, nên VM đạt hiệu năng gần máy thật
- **Ví dụ:** 1 máy chủ 32 CPU, 128 GB RAM chạy được **10–15 VM** (web, database, mail). Tiết kiệm phần cứng, điện, chỗ đặt máy; tạo máy mới chỉ vài phút

<h2 class="!mt-3 !mb-1">Hai điểm thú vị</h2>

- Hyper-V thực chất là <b class="lpc-ring">Type 1</b>: Windows thành một phân vùng chạy trên hypervisor
- RHEL cũng làm hypervisor: **KVM** (trong kernel) + **QEMU** (giả lập phần cứng) + **libvirt** (quản lý). Nitro dưới EC2 của AWS cũng dựa trên KVM

::right::

<h2 class="!m-0">Hai loại hypervisor</h2>

<div class="lpc-mini mt-2">
  <h3>Type 1 · bare-metal</h3>
  <p>Chạy trực tiếp trên phần cứng: VMware ESXi, Microsoft Hyper-V, KVM. Dùng trong datacenter, cloud.</p>
</div>

<div class="lpc-mini mt-2">
  <h3>Type 2 · hosted</h3>
  <p>Chạy như ứng dụng trên OS có sẵn: VirtualBox, VMware Workstation. Hợp để học tập, thử nghiệm.</p>
</div>

<!--
Đây là nền móng của mọi phần phía sau: trước khi có một máy chủ RHEL để cấu hình, ta cần có máy, và trong doanh nghiệp máy đó hầu hết là máy ảo.
Mỗi VM tưởng rằng mình đang chạy trên một máy thật riêng.
Khi bật Hyper-V, hypervisor được nạp xuống dưới cùng, còn Windows trở thành một phân vùng chạy trên nó.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">1. Hypervisor và vòng đời VM</span>

# Một VM gồm những gì

<TitleLine width="260px" />

- Một nhóm file: **file cấu hình** (vCPU, RAM, card mạng) + **file ổ đĩa ảo** (OS và dữ liệu)
- Định dạng đĩa: VDI (VirtualBox) · VHDX (Hyper-V) · VMDK (VMware) · QCOW2 (KVM)
- **Thin**: lớn dần theo dữ liệu, tiết kiệm chỗ · **Thick**: chiếm đủ từ đầu, ổn định hơn
- **Overcommit**: tổng vCPU / RAM cấp cho VM vượt tài nguyên thật. Quá tay thì các VM cùng chậm

<LessonCard class="mt-2">

RHEL 10 cần CPU **x86-64-v3** (AVX2, Intel Haswell / AMD Excavator trở đi). Hypervisor phải chuyển tính năng này vào VM, nếu không RHEL 10 không khởi động và báo **lỗi glibc**.

</LessonCard>

::right::

<h2 class="!m-0">Vòng đời của một VM</h2>

<div class="lpc-flow">
  <span class="lpc-chip">Tạo</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Cài đặt, cấu hình</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Vận hành</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Bảo trì</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">Thu hồi</span>
</div>

- **Bảo trì**: snapshot trước khi cập nhật, backup định kỳ, clone máy mới
- **Thu hồi**: backup lần cuối nếu cần rồi xóa. Với RHEL, **hủy đăng ký subscription** trước khi xóa
- Doanh nghiệp tạo **template** rồi clone hàng loạt. Clone máy Linux phải làm sạch **machine-id, SSH host key, hostname**
- Type 1 hỗ trợ **live migration**: chuyển VM đang chạy sang máy vật lý khác, không phải tắt

<!--
Vì VM chỉ là file nên có thể sao chép, di chuyển, sao lưu dễ dàng. Overcommit được vì không phải VM nào cũng dùng hết cùng lúc.
Ví dụ x86-64-v3 cho thấy cấu hình hypervisor ảnh hưởng trực tiếp đến hệ điều hành bên trong.
Giai đoạn cài đặt và cấu hình chính là nội dung các phần 2 đến 5. Vận hành là theo dõi tài nguyên, điều chỉnh khi thiếu như tăng RAM hay thêm ổ.
-->

---
layout: side-panel
label: Snapshot
---

::left::

<p class="!text-xl !m-0 italic text-[var(--lpc-muted)]">Như nút "Undo" trong Word</p>

- Lưu trạng thái VM tại một thời điểm
- Ổ gốc "đóng băng", thay đổi mới ghi vào **file chênh lệch**
- Quay về: bỏ file chênh lệch, gần như tức thì
- Phụ thuộc ổ gốc, nằm cạnh VM: hỏng ổ cứng máy chủ là **mất cả VM lẫn snapshot**

::default::

<span class="lpc-tag">1. Hypervisor và vòng đời VM</span>

# Snapshot khác backup ở điểm nào

<TitleLine width="200px" />

<div v-click>

<h2 class="!m-0">Backup</h2>
<p class="!text-xl !mt-1 italic text-[var(--lpc-muted)]">Như bản sao cất trong két sắt ở nơi khác</p>

- Bản sao **đầy đủ, độc lập, lưu ở nơi khác**: khôi phục được kể cả khi mất toàn bộ máy chủ
- Nguyên tắc **3-2-1**: 3 bản dữ liệu · 2 loại thiết bị · 1 bản ở nơi khác

<p class="!text-lg !mt-3"><b>Ví dụ:</b> snapshot trước <code>dnf update</code>. Lỗi thì quay lại vài giây. Thành công thì xóa snapshot (giữ lâu file chênh lệch phình to, VM chậm dần). Backup vẫn chạy hằng đêm.</p>

</div>

<!--
Snapshot và backup bổ sung cho nhau: snapshot để quay lại nhanh trước một thay đổi, backup để khôi phục khi mất cả máy chủ.
Khi đã có máy, câu hỏi tiếp theo là làm sao để các máy kết nối được với nhau và ra Internet.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">2. Network: IP, routing, DNS và kết nối giữa các VM</span>

# Routing và gateway

<TitleLine width="260px" />

- Mỗi máy cần **IP** và **subnet mask** (viết gọn kiểu CIDR)
- `192.168.56.10/24`: 24 bit mạng, 8 bit máy → 256 địa chỉ (.0 mạng, .255 broadcast, **254 dùng được**)
- 3 dải private, không đi thẳng ra Internet: `10.0.0.0/8` · `172.16.0.0/12` · `192.168.0.0/16`
- Đích **cùng mạng**: gửi thẳng, dùng **ARP** tìm MAC. Đích **khác mạng**: gửi cho **gateway** (router)
- Bảng định tuyến: chọn route **cụ thể nhất** (prefix dài nhất). Không khớp thì đi **default route**. Nhiều route cùng đích thì **metric** nhỏ hơn được ưu tiên

::right::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)]">Ví dụ: máy có hai card mạng</p>

```text
$ ip route
default via 10.0.2.2 dev enp0s3
    proto dhcp metric 100
192.168.56.0/24 dev enp0s8 proto kernel
    scope link src 192.168.56.10 metric 101
```

- Đến `192.168.56.0/24`: đi thẳng qua **enp0s8**, không cần gateway
- Mọi đích khác, kể cả Internet: theo default route qua gateway **10.0.2.2** trên enp0s3

<LessonCard class="mt-2">

Thông thường chỉ card ra Internet mới khai báo gateway, tránh để máy có **hai default route** gây nhầm đường.

</LessonCard>

<!--
Khi gửi gói tin, máy so IP đích với mạng của mình. Nếu đích ở mạng khác, gói tin được gửi cho gateway chuyển tiếp. Quy tắc chọn đường nằm trong bảng định tuyến.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">2. Network: IP, routing, DNS và kết nối giữa các VM</span>

# Các chế độ mạng của VM

<TitleLine width="260px" />

<div class="grid grid-cols-2 gap-2">
  <div class="lpc-mini"><h3>NAT</h3><p>VM ra được Internet, bên ngoài không truy cập vào được</p></div>
  <div class="lpc-mini"><h3>Host-only</h3><p>Máy thật và các VM thấy nhau, không ra Internet</p></div>
  <div class="lpc-mini"><h3>Bridged</h3><p>Như máy thật trong LAN, nhận IP cùng dải</p></div>
  <div class="lpc-mini"><h3>Internal</h3><p>Chỉ các VM nói chuyện với nhau</p></div>
</div>

- Trên Hyper-V: **External, Internal, Private switch**
- Bố trí phổ biến: mỗi VM hai card, **1 card NAT** để tải phần mềm, **1 card mạng nội bộ** để quản trị và kết nối giữa các máy

::right::

<h2 class="!m-0">Quản lý mạng trên RHEL 10</h2>

- **NetworkManager** quản lý mạng. Mỗi cấu hình là một **connection profile** gắn với một card mạng
- RHEL 10 **bỏ hẳn định dạng ifcfg** trong `/etc/sysconfig/network-scripts/`
- Profile lưu dạng **keyfile** (`.nmconnection`) trong `/etc/NetworkManager/system-connections/`
- Cấu hình bằng **nmcli**, **nmtui** hoặc **Cockpit**

```bash
# Ví dụ: đặt IP tĩnh và DNS
nmcli con mod enp0s8 ipv4.method manual \
  ipv4.addresses 192.168.56.10/24
nmcli con mod enp0s8 ipv4.dns "8.8.8.8 1.1.1.1"
nmcli con up enp0s8
```

<!--
Hypervisor cho VM nhiều chế độ mạng, mỗi chế độ trả lời một câu hỏi khác nhau.
Vì RHEL 10 bỏ ifcfg, các tài liệu cũ hướng dẫn sửa file ifcfg-eth0 không còn áp dụng được.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">2. Network: IP, routing, DNS và kết nối giữa các VM</span>

# DNS và phân giải tên

<TitleLine width="260px" />

- DNS chuyển **tên thành IP**
- Thứ tự tra tên khai báo trong `/etc/nsswitch.conf`: thường **/etc/hosts trước, DNS sau**
- `/etc/resolv.conf` chứa địa chỉ DNS server nhưng do **NetworkManager tự sinh**: khai báo DNS phải qua NetworkManager, sửa tay sẽ bị ghi đè
- Bản ghi cơ bản: **A** (tên → IPv4) · **AAAA** (tên → IPv6) · **CNAME** (bí danh) · **PTR** (IP → tên) · **MX** (máy chủ mail)

::right::

<h2 class="!m-0">Kiểm tra khi mất kết nối</h2>
<p class="!mt-1 !mb-2">Đi từ lớp dưới lên:</p>

<v-clicks>

<ol class="lpc-steps">
  <li><span>Card mạng đã có IP chưa? <code>ip addr</code></span></li>
  <li><span>Có đường đi chưa? <code>ip route</code></span></li>
  <li><span>Ping được gateway không?</span></li>
  <li><span>Ping được IP đích không?</span></li>
  <li><span>Phân giải tên (DNS)</span></li>
  <li><span>Cổng dịch vụ: <code>ss</code>, <code>curl</code></span></li>
</ol>

</v-clicks>

<p class="!mt-2"><b>Ví dụ:</b> ping <code>8.8.8.8</code> được mà ping <code>google.com</code> không được → lỗi ở <b>DNS</b>, không phải đường mạng.</p>

<!--
Con người nhớ tên, máy tính dùng IP, nên DNS làm nhiệm vụ chuyển tên thành IP.
Điều quan trọng nhất em rút ra ở phần network là thứ tự kiểm tra khi mất kết nối, đi từ lớp dưới lên.
Khi mạng đã thông, việc tiếp theo là chuẩn bị chỗ lưu dữ liệu cho máy chủ.
-->

---
layout: title-right
class: dense
---

::left::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)] text-center">Các lớp lưu trữ</p>

<div class="lvm">
  <div class="lvm-box wide">Ổ đĩa (block device trong /dev)</div>
  <div class="lvm-arrow">↓</div>
  <div class="lvm-box wide">Partition (MBR / GPT)</div>
  <div class="lvm-arrow">↓</div>
  <div class="lvm-box wide">LVM (tùy chọn): PV → VG → LV</div>
  <div class="lvm-arrow">↓</div>
  <div class="lvm-box wide">Filesystem</div>
  <div class="lvm-arrow">↓</div>
  <div class="lvm-box wide navy">Mount vào một thư mục</div>
</div>

<h2 class="!mt-3 !mb-1">LVM: PV, VG, LV</h2>

- **PV**: ổ đĩa hoặc partition thật
- **VG**: "bể chứa" gom nhiều PV, chia thành **extent** (mặc định 4 MiB)
- **LV**: phần cắt ra từ bể, dùng như một ổ đĩa
- Hết chỗ: thêm ổ vào VG rồi nới LV, không phải chia lại ổ

<style>
.lvm { display:flex; flex-direction:column; align-items:center; gap:2px; margin:6px 0 4px; }
.lvm-box { background:var(--lpc-soft); border:2px solid var(--lpc-navy); border-radius:10px; padding:4px 12px; font-size:1.02rem; text-align:center; }
.lvm-box.wide { width:420px; }
.lvm-box.navy { background:var(--lpc-navy); color:#fff; }
.lvm-arrow { color:var(--lpc-orange); font-weight:800; font-size:1.1rem; line-height:1; }
</style>

::default::

<span class="lpc-tag">3. Storage: partition, LVM, filesystem, mount</span>

# Ổ đĩa và partition

<TitleLine width="200px" />

- **Không để dữ liệu ứng dụng chung phân vùng với OS**: log hoặc dữ liệu làm đầy ổ thì cả máy chủ có thể ngừng
- Mỗi ổ là một block device: `/dev/sda`, `/dev/sdb` (SATA/SCSI) · `/dev/nvme0n1` (NVMe) · `/dev/vda` (ổ ảo virtio trên KVM)
- **MBR** (cũ): tối đa ổ 2 TB, 4 partition chính
- **GPT** (mới): ổ rất lớn, mặc định tới 128 partition, đi kèm UEFI
- Công cụ: fdisk, gdisk, parted

<div class="lpc-mini mt-2">
  <h3>Trên RHEL</h3>
  <p>Cài với phân vùng tự động: OS nằm trên LVM, VG <b>"rhel"</b> với LV root và swap. Từ RHEL 9 có <b>LVM devices file</b> (<code>/etc/lvm/devices/system.devices</code>): LVM chỉ làm việc với thiết bị có tên trong file, tránh nhận nhầm ổ của hệ thống khác.</p>
</div>

<!--
Lợi ích lớn nhất của LVM là linh hoạt: khi hết chỗ chỉ cần thêm ổ vào VG rồi nới rộng LV.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">3. Storage: partition, LVM, filesystem, mount</span>

# Filesystem và mount

<TitleLine width="260px" />

- **XFS**: mặc định của RHEL 10, mạnh với file lớn và ghi song song, **chỉ mở rộng được**, không thu nhỏ
- **ext4** vẫn được hỗ trợ (thu nhỏ được khi đã umount). **Btrfs không được hỗ trợ**
- Lệnh `mount` chỉ có hiệu lực đến lần khởi động lại. Muốn tự động thì khai báo trong **/etc/fstab**
- Mỗi dòng fstab 6 trường: thiết bị · điểm mount · loại filesystem · tùy chọn · dump · thứ tự fsck
- Dùng **UUID** thay tên thiết bị, vì `/dev/sdb` có thể đổi khi thêm bớt ổ

<LessonCard class="mt-2">

Một dòng sai trong fstab có thể khiến máy vào **emergency mode**. Kiểm tra bằng `mount -a` trước khi reboot.

</LessonCard>

::right::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)]">Ví dụ: tạo vùng lưu trữ /data từ ổ mới</p>

```bash
pvcreate /dev/sdb1                # khởi tạo PV
vgcreate vg_data /dev/sdb1        # tạo VG
lvcreate -n lv_web -L 5G vg_data  # tạo LV 5 GB
mkfs.xfs /dev/vg_data/lv_web      # tạo XFS
# /etc/fstab: UUID=<uuid> /data xfs defaults 0 0
# nới LV và filesystem:
lvextend -r -L +3G /dev/vg_data/lv_web
```

<div class="lpc-mini mt-3">
  <h3>Swap</h3>
  <p>Vùng đĩa dùng làm "RAM dự phòng" khi RAM thật đầy. Giúp máy không bị treo ngay khi thiếu RAM, nhưng <b>chậm hơn RAM rất nhiều</b>.</p>
</div>

<!--
Có chỗ chứa dữ liệu rồi, câu hỏi tiếp theo là ai được phép đọc, ghi vào đó.
-->

---
layout: title-left-card
---

<span class="lpc-tag">4. User, group và permission</span>

# User và group

<TitleLine width="260px" />

- Linux là hệ điều hành **nhiều người dùng**, mỗi tài khoản có mã số **UID**:
  - root: UID **0**, toàn quyền
  - tài khoản hệ thống cho dịch vụ (như apache): UID **dưới 1000**
  - người dùng thường: từ **1000**
- Mỗi user có một **nhóm chính** (RHEL tự tạo nhóm trùng tên) và có thể thuộc nhiều **nhóm phụ** để chia sẻ quyền

::right::

<h2 class="!m-0">Thông tin lưu trong 3 file</h2>

| File | Nội dung |
|---|---|
| `/etc/passwd` | Tên, UID, GID, home, shell |
| `/etc/shadow` | Mật khẩu đã băm, chính sách hết hạn. Chỉ root đọc được |
| `/etc/group` | Danh sách nhóm và thành viên |

<div class="lpc-mini mt-4" style="border-top-color: var(--lpc-orange)">
  <h3>Điểm riêng của RHEL 10</h3>
  <p>Trình cài đặt mặc định <b>khóa tài khoản root</b>, khuyến khích tạo user quản trị thuộc nhóm <b>wheel</b>, làm việc qua <b>sudo</b>. An toàn hơn đăng nhập thẳng bằng root.</p>
</div>

<!--
Mỗi tài khoản có một mã số UID. Thông tin người dùng được lưu trong ba file passwd, shadow và group.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">4. User, group và permission</span>

# Quyền truy cập file

<TitleLine width="260px" />

- Mỗi file: **owner (u) · group (g) · other (o)**, quyền **r = 4 · w = 2 · x = 1**
- Với thư mục: r là xem danh sách file, w là tạo hoặc xóa file, x là được đi vào
- Ví dụ `750`: owner rwx, group r-x, other không có quyền
- **umask** quyết định quyền mặc định: umask 022 → file 644, thư mục 755

<div class="grid grid-cols-3 gap-2 mt-2">
  <div class="lpc-mini"><h3>setuid (4)</h3><p>Chạy với quyền chủ file, ví dụ lệnh passwd</p></div>
  <div class="lpc-mini"><h3>setgid (2)</h3><p>Trên thư mục: file mới thuộc nhóm của thư mục</p></div>
  <div class="lpc-mini"><h3>sticky (1)</h3><p>Chỉ chủ file xóa được file, ví dụ /tmp 1777</p></div>
</div>

- Cần chi tiết hơn mô hình u/g/o: dùng **ACL** (setfacl, getfacl)

<LessonCard class="mt-3">

`-a` để thêm nhóm, không ghi đè nhóm cũ. Sửa sudoers bằng **visudo** (kiểm tra cú pháp).

</LessonCard>


::right::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)]">Ví dụ: An và Bình cùng làm /data/web</p>

```bash
groupadd webteam
usermod -aG webteam an
chown root:webteam /data/web
chmod 2770 /data/web
# drwxrws---  root webteam  /data/web
```

<p class="!mt-1 !mb-0">Số <b class="lpc-ring">2</b> (setgid): file mới thuộc webteam, hai bạn sửa được file của nhau.</p>

<h2 class="!mt-2 !mb-1">sudo và least privilege</h2>

- Nhóm wheel dùng sudo toàn quyền. Least privilege: **chỉ cấp đúng quyền cần thiết**
- Ví dụ: team web chỉ được restart httpd

```bash
# /etc/sudoers.d/webteam
%webteam ALL=(root) /usr/bin/systemctl restart httpd
```


<!--
Mọi file đều trả lời ba câu hỏi: ai sở hữu, thuộc nhóm nào, và những người còn lại được làm gì.
Tình huống ví dụ: team web gồm An và Bình cần cùng làm việc trong /data/web, người ngoài nhóm không được xem.
Team web chỉ cần khởi động lại dịch vụ web thì chỉ cấp đúng lệnh đó. Dịch vụ httpd nhắc đến ở đây là nội dung phần tiếp theo.
-->

---
layout: title-right
class: dense
---

::left::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)]">Các loại unit hay gặp</p>

| Unit | Ý nghĩa |
|---|---|
| `.service` | Dịch vụ |
| `.socket` | Socket |
| `.timer` | Lập lịch, thay thế cron |
| `.mount` | Điểm mount, sinh ra từ fstab |
| `.target` | Nhóm unit, tương đương runlevel |

<p class="!mt-1 text-[var(--lpc-muted)]">multi-user.target: máy chủ không giao diện · graphical.target: có giao diện đồ họa</p>

```bash
# Ví dụ
systemctl enable --now httpd  # chạy + tự khởi động
systemctl status httpd        # trạng thái, ít log
journalctl -u httpd -n 50     # 50 dòng log
```

::default::

<span class="lpc-tag">5. Service (systemd) và firewall</span>

# Service (systemd)

<TitleLine width="200px" />

- **systemd** là tiến trình đầu tiên khi khởi động (**PID 1**), khởi động và quản lý mọi dịch vụ theo đơn vị **unit**
- `start`: chạy ngay bây giờ · `enable`: tự chạy mỗi khi khởi động · cả hai: `enable --now`
- Unit của gói phần mềm: `/usr/lib/systemd/system/`. Unit của quản trị viên: `/etc/systemd/system/`, **được ưu tiên hơn**
- Sửa unit file xong phải chạy `systemctl daemon-reload`
- Log do **journald** thu thập, xem bằng `journalctl`
- Trong unit file: `Restart=on-failure` tự khởi động lại khi lỗi · `User=` chạy bằng user thường thay vì root

<!--
systemd quản lý theo đơn vị gọi là unit. Hai khái niệm dễ nhầm là start và enable.
-->

---
layout: side-panel
label: Web không vào được?
class: dense
---

::left::

<p class="!text-xl !m-0 font-600">Có thể do một trong bốn lớp, kiểm tra lần lượt:</p>

<ol class="lpc-steps !mt-3">
  <li><span>Dịch vụ đã dừng</span></li>
  <li><span>Firewall chặn</span></li>
  <li><span>Sai quyền file</span></li>
  <li><span>SELinux chặn</span></li>
</ol>

<p class="!mt-2">Giống cách kiểm tra mạng ở phần 2.</p>

::default::

<span class="lpc-tag">5. Service (systemd) và firewall</span>

# Firewall với firewalld

<TitleLine width="200px" />

- Firewall mặc định của RHEL, dùng **nftables** trong kernel làm nền
- Tổ chức theo **zone**: mỗi card thuộc một zone (mặc định **public**), chỉ service/port được phép mới vào
- **Runtime**: hiệu lực ngay, mất khi reload/reboot · **Permanent**: lưu lại, hiệu lực sau reload → thêm `--permanent` rồi `--reload`

```bash
firewall-cmd --permanent --add-service=http
firewall-cmd --reload
firewall-cmd --list-all
```

<h2 class="!mt-2 !mb-1">SELinux: lớp bảo vệ thứ ba</h2>

- Mặc định **enforcing**. Mỗi tiến trình và file mang một **nhãn** (context)
- `httpd_t` chỉ đọc được file nhãn `httpd_sys_content_t`. Đổi thư mục web sang `/data/web` → **lỗi 403** dù rwx đúng
- Xử lý: gán lại nhãn bằng `semanage fcontext` và `restorecon`, **không tắt SELinux**. Lần bị chặn ghi trong **log AVC**

<!--
Ngoài quyền file ở phần 4 và firewall, RHEL còn có SELinux. Chính sách SELinux quy định nhãn tiến trình nào được truy cập nhãn file nào.
Khi hệ thống đã chạy ổn, việc tiếp theo là theo dõi xem máy chủ có khỏe không.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">6. Linux doanh nghiệp: quản lý tài nguyên máy chủ</span>

# CPU và load average

<TitleLine width="260px" />

<p class="!m-0">Trả lời 4 câu hỏi theo thứ tự: CPU có quá tải? RAM có đủ? Disk còn chỗ, có nghẽn? Tiến trình nào gây vấn đề?</p>

- **Load average**: số tiến trình trung bình đang chạy hoặc chờ tài nguyên (CPU hoặc I/O không thể ngắt), trong **1, 5, 15 phút**
- Phải **so với số CPU**. Số 1 phút lớn hơn số 15 phút: tải đang tăng
- Dòng `%Cpu` trong top: **us** (ứng dụng) · **sy** (kernel) · **wa** (chờ disk) · **id** (rảnh) · **st** (hypervisor lấy cho VM khác, liên quan overcommit)
- **wa cao**: nghẽn ở disk, không phải CPU

<div class="text-center mt-2">
  <p class="!m-0 text-[var(--lpc-muted)]">Ví dụ: load trên máy <b>2 CPU</b></p>
  <p class="bignum !m-0 font-800 text-[var(--lpc-navy)]">3.95</p>
  <p class="!m-0">Công việc gần <b>gấp đôi</b> khả năng xử lý → quá tải</p>
</div>

::right::

<h2 class="!m-0">RAM và swap</h2>

```text
$ free -h
      total  used   free   buff/cache  available
Mem:  1.9Gi  620Mi  110Mi  1.2Gi       1.1Gi
```

- Cột free chỉ 110 MB nhưng **không thiếu RAM**
- Linux tận dụng RAM trống làm **cache** (buff/cache) và nhả ra khi ứng dụng cần
- Con số cần nhìn: **available** (1.1 GB)
- Dấu hiệu thiếu RAM thật: **swap tăng liên tục**, hoặc kernel kích hoạt **OOM killer** để "giết" tiến trình

<!--
Khi có báo cáo máy chủ chạy chậm, nên kiểm tra theo đúng thứ tự CPU, RAM, disk, tiến trình.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">6. Linux doanh nghiệp: quản lý tài nguyên máy chủ</span>

# Disk và tiến trình

<TitleLine width="260px" />

<h2 class="!m-0">Disk</h2>

- Hai vấn đề khác nhau: **hết dung lượng** và **nghẽn tốc độ**
- `df -h`: phân vùng nào đầy · `du -sh`: thư mục nào chiếm chỗ · `iostat`: tốc độ đọc ghi
- Quá nhiều file nhỏ có thể **hết inode** (`df -i`), không ghi được file mới dù còn dung lượng

<h2 class="!mt-2 !mb-0">Tiến trình, tín hiệu và độ ưu tiên</h2>

- Mỗi tiến trình có **PID**, xem bằng ps hoặc top
- **SIGTERM (15)**: dừng nhẹ nhàng, thử trước · **SIGKILL (9)**: buộc dừng, khi không phản hồi
- **nice** từ -20 (ưu tiên cao nhất) đến 19 (thấp nhất), mặc định 0. Backup nên chạy nice cao. User thường chỉ được giảm ưu tiên

::right::

<h2 class="!m-0">cgroups v2 và công cụ quản lý của RHEL</h2>

- systemd đặt mỗi dịch vụ vào một **cgroup** để theo dõi và giới hạn tài nguyên
- **RHEL 10 chỉ còn cgroups v2**, đã bỏ v1

```bash
# Ví dụ: tối đa nửa CPU và 256 MB RAM
systemctl set-property hello-app.service \
  CPUQuota=50% MemoryMax=256M
```

<div class="grid grid-cols-3 gap-2 mt-2">
  <div class="lpc-mini"><h3>Cockpit</h3><p>Giao diện web quản trị, cổng 9090</p></div>
  <div class="lpc-mini"><h3>tuned</h3><p>Tối ưu theo profile, ví dụ virtual-guest</p></div>
  <div class="lpc-mini"><h3>sos report</h3><p>Thu thập thông tin gửi hỗ trợ Red Hat</p></div>
</div>

<p class="!mt-2">cgroups cũng là nền tảng Docker dùng để giới hạn tài nguyên container.</p>

<!--
SIGTERM cho tiến trình kịp lưu dữ liệu và dọn dẹp. Muốn tăng ưu tiên phải có quyền root.
Cơ chế cgroups vừa nói cũng là nền tảng Docker dùng để giới hạn tài nguyên cho container, nội dung của phần cuối cùng.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">7. Container</span>

# Container khác VM ở điểm nào

<TitleLine width="260px" />

<p class="!m-0">Container <b>không phải "máy ảo nhỏ"</b> mà là <b>tiến trình Linux được cô lập</b>, xây từ tính năng của kernel.</p>

<div class="lpc-mini mt-2">
  <h3>VM · như căn nhà riêng</h3>
  <p>Ảo hóa phần cứng. Kernel và OS riêng, nặng vài GB, khởi động cả phút.</p>
</div>

<div v-click class="lpc-mini mt-2" style="border-top-color: var(--lpc-orange)">
  <h3>Container · như căn hộ chung cư</h3>
  <p>Ảo hóa mức OS, dùng chung kernel. Vài chục đến vài trăm MB, khởi động vài giây. <b>Namespaces</b> (pid, net, mnt, uts, ipc, user) cô lập; <b>cgroups v2</b> giới hạn CPU, RAM.</p>
</div>

- Đổi lại: cô lập kém hơn VM (lỗ hổng kernel ảnh hưởng mọi container), cần kernel Linux. Doanh nghiệp thường **chạy container bên trong VM**

::right::

<h2 class="!m-0">Kiến trúc Docker</h2>

<div class="lpc-flow">
  <span class="lpc-chip">docker (CLI)</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">dockerd</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">containerd</span><span class="lpc-arrow">→</span>
  <span class="lpc-chip">runc</span>
</div>

- **dockerd** là một dịch vụ systemd (phần 5). **runc** thực sự tạo namespaces và cgroups
- Image và container theo chuẩn mở **OCI**: chạy được trên Podman hay Kubernetes
- Trên RHEL, công cụ mặc định là **Podman** (không cần daemon, chạy được không cần root). Docker cài từ repo chính thức của Docker cho RHEL

<LessonCard class="mt-2">

**Nhóm docker** có quyền gần như root: chỉ thêm người thật sự cần.

</LessonCard>

<!--
Điều quan trọng nhất em hiểu ở phần này: container là tiến trình Linux được cô lập, xây dựng từ chính các tính năng của kernel đã nhắc ở các phần trước.
Có thể hình dung VM như những căn nhà riêng, mỗi nhà có móng riêng. Container giống các căn hộ chung cư dùng chung móng là kernel nhưng mỗi căn có cửa khóa riêng.
Thành viên nhóm docker điều khiển được dockerd đang chạy bằng root.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">7. Container</span>

# Image

<TitleLine width="260px" />

- Bản đóng gói **chỉ đọc** gồm ứng dụng, thư viện và cấu hình
- Mỗi lệnh Dockerfile tạo một **layer**, layer giống nhau dùng chung giữa các image
- Khi chạy, Docker thêm **lớp ghi mỏng** trên cùng: một image chạy được nhiều container
- **Cache layer**: đặt bước ít thay đổi (cài thư viện) trước, bước hay thay đổi (copy code) sau
- Lệnh cơ bản: FROM · RUN · COPY · ENV · EXPOSE · USER · CMD/ENTRYPOINT

```dockerfile
# Ví dụ: đóng gói trang web tĩnh
FROM nginx:1.27-alpine
COPY site/ /usr/share/nginx/html/
EXPOSE 80
# build:  docker build -t mysite:1.0 .
```

::right::

<div class="lpc-mini !mb-3" style="border-top-color: var(--lpc-orange)">
  <h3>Nguyên tắc tốt</h3>
  <p>Image nền nhỏ · gắn phiên bản cụ thể (nginx:1.27) thay vì latest · không chạy bằng root nếu không cần · không đưa mật khẩu vào image. Red Hat có <b>UBI</b>, image nền dựa trên RHEL, phân phối tự do.</p>
</div>

<h2 class="!m-0">Registry</h2>

- Kho image: Docker Hub · registry.redhat.io, Quay.io · Amazon ECR · nội bộ
- Tên đầy đủ: `registry/namespace/tên:tag`, ví dụ `docker.io/library/nginx:1.27`. Không ghi registry thì mặc định Docker Hub
- **Digest** (sha256) cố định, đảm bảo kéo đúng bản đã kiểm duyệt, vì tag có thể bị gắn lại
- Quy trình: build → `docker tag` → `docker push` → máy khác `docker pull`

<!--
Image bên dưới không bị sửa khi chạy container, nhờ đó một image chạy được nhiều container. Bước nào không đổi thì lần build sau dùng lại cache.
Registry giống kho ứng dụng cho container.
-->

---
layout: title-right
class: dense
---

::left::

<p class="!text-lg !m-0 font-600 text-[var(--lpc-navy)] text-center">Container trong VM RHEL truy cập Internet</p>

<div class="net">
  <div class="net-box">Container "web" · eth0 <b>172.17.0.2</b></div>
  <div v-click class="net-step">
    <div class="net-arrow">↓ cặp veth, gửi tới gateway 172.17.0.1</div>
    <div class="net-box">Bridge <b>docker0</b> (switch ảo trong VM) · 172.17.0.1</div>
  </div>
  <div v-click class="net-step">
    <div class="net-arrow">↓ <b>NAT lần 1</b>: MASQUERADE 172.17.0.2 → 10.0.2.15</div>
    <div class="net-box">VM RHEL 10.2 (web01) · enp0s3 <b>10.0.2.15</b> (NAT) · enp0s8 192.168.56.10</div>
  </div>
  <div v-click class="net-step">
    <div class="net-arrow">↓ <b>NAT lần 2</b>: hypervisor (VirtualBox NAT / Hyper-V WinNAT)</div>
    <div class="net-box">Laptop Windows · Wi-Fi <b>192.168.1.20</b></div>
  </div>
  <div v-click class="net-step">
    <div class="net-arrow">↓ <b>NAT lần 3</b>: router đổi sang IP public</div>
    <div class="net-box navy">Router → nhà mạng → Internet</div>
  </div>
</div>

<style>
.net { display:flex; flex-direction:column; align-items:center; margin-top:6px; }
.net-step { display:flex; flex-direction:column; align-items:center; }
.net-box { width:470px; background:var(--lpc-soft); border:2px solid var(--lpc-navy); border-radius:10px; padding:5px 10px; font-size:1rem; text-align:center; }
.net-box.navy { background:var(--lpc-navy); color:#fff; }
.net-box.navy b { color:#fff; }
.net-arrow { color:var(--lpc-orange); font-weight:700; font-size:0.95rem; line-height:1.5; }
.net-arrow b { color:var(--lpc-orange); }
</style>

::default::

<span class="lpc-tag">7. Container</span>

# Network

<TitleLine width="200px" />

- Driver: **bridge** (mặc định) · host · none · overlay · macvlan
- Bridge: container có **network namespace** riêng, cặp **veth** nối eth0 vào **docker0**; docker0 là gateway, VM đóng vai trò router
- **4 chặng, 3 lần NAT** (thêm 1 nếu nhà mạng dùng CGNAT). Gói trả lời đi ngược lại nhờ **conntrack**
- Từ ngoài vào phải qua **publish port**: `-p 8080:80` là rule **DNAT**
- Cùng **user-defined network**: nói chuyện trực tiếp, gọi nhau bằng tên qua DNS nội bộ **127.0.0.11** (docker0 mặc định không có)

<LessonCard class="mt-1">

Docker tạo **zone docker** trong firewalld: cổng publish có thể mở ra ngoài dù chưa khai báo. Giới hạn: `-p 127.0.0.1:8080:80`.

</LessonCard>

<!--
Khi tạo container kiểu bridge, Docker tạo network namespace riêng có eth0, IP và bảng route; tạo cặp veth như sợi dây mạng ảo hai đầu; và gán docker0 địa chỉ 172.17.0.1 làm gateway.
Chặng 1: IP đích không cùng mạng 172.17.0.0/16 nên gửi cho docker0. Chặng 2: kernel VM định tuyến theo default route (Docker bật ip_forward), MASQUERADE đổi IP nguồn thành IP của VM. Từ đây bên ngoài chỉ thấy VM. Chặng 3 và 4: hypervisor rồi router tiếp tục NAT.
Mạng 172.17.x.x chỉ tồn tại trong VM. Khi container kết nối tới db01 (192.168.56.11), db01 thấy kết nối đến từ IP của VM chứa container.
Ví dụ: docker network create appnet; chạy db và web cùng mạng appnet, web gọi db bằng tên "db", database dùng volume pgdata.
-->

---
layout: title-right
image: /images/port-containers.jpg
class: dense
---

<span class="lpc-tag">7. Container</span>

# Volume

<TitleLine width="200px" />

- Xóa container là **mất lớp ghi**: dữ liệu lâu dài đặt ra ngoài
- **Named volume**: Docker quản lý (`/var/lib/docker/volumes/`), hợp cho database
- **Bind mount**: gắn thư mục có sẵn của máy chủ, ví dụ `/data/web`
- **tmpfs**: lưu tạm trên RAM, mất khi container dừng
- Trên RHEL, bind mount cần `:Z` (riêng 1 container) hoặc `:z` (dùng chung) cho SELinux

<h2 class="!mt-2 !mb-1">Resource limit</h2>

- Mặc định **không giới hạn**: một container lỗi có thể chiếm hết RAM, CPU
- `--memory` (vượt thì **OOMKilled**) · `--cpus` · `--pids-limit` · `--restart`. Theo dõi: `docker stats`

```bash
docker run -d --name web --memory 256m \
  --cpus 0.5 --restart on-failure mysite:1.0
```

<!--
Docker dùng cgroups v2, cùng cơ chế systemd dùng ở phần 6, để giới hạn. Restart policy tương tự Restart= của systemd.
Các khái niệm image, registry, volume, network, resource limit cũng chính là nền tảng của Kubernetes.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">Các mảng kiến thức kết nối với nhau như thế nào</span>

# Tổng kết

<TitleLine width="260px" />

- Bảy mảng là **các lớp chồng lên nhau** của cùng một hệ thống, lớp dưới là tiền đề cho lớp trên
- Ví dụ: VM không được cấp x86-64-v3 thì RHEL 10 không khởi động

<p class="!mt-2 !mb-1 font-700 text-[var(--lpc-navy)]">3 khái niệm xuyên suốt</p>

<div class="lpc-mini !mb-2"><h3>cgroups</h3><p>systemd giới hạn dịch vụ, Docker giới hạn container</p></div>
<div class="lpc-mini !mb-2"><h3>Routing và NAT</h3><p>Cho VM, cho container (docker0, MASQUERADE, publish port) và trên cloud</p></div>
<div class="lpc-mini"><h3>Phân quyền nhiều lớp</h3><p>Quyền file, sudo, SELinux, nhóm docker gần như root</p></div>

::right::

<h2 class="!m-0">4 nguyên tắc tư duy</h2>

<v-clicks>

<ol class="lpc-steps">
  <li><span><b>Nhìn hệ thống theo lớp</b></span></li>
  <li><span><b>Kiểm tra từ dưới lên</b>: IP, route, gateway, DNS, cổng, ứng dụng</span></li>
  <li><span><b>Đọc log trước khi sửa</b>: journalctl, systemctl status, log AVC</span></li>
  <li><span><b>Luôn có đường lui</b>: snapshot, backup, kiểm tra cấu hình trước reboot</span></li>
</ol>

</v-clicks>

<div class="lpc-mini mt-2" style="border-top-color: var(--lpc-orange)">
  <h3>Tự đánh giá</h3>
  <p>Nắm lý thuyết nền tảng, hiểu "vì sao". Kinh nghiệm thực hành <b>còn rất ít</b>. Ưu tiên số một: <b>biến lý thuyết thành kỹ năng</b>.</p>
</div>

<!--
Đi từ dưới lên: hypervisor cung cấp phần cứng ảo cho máy chủ RHEL. Network giúp máy giao tiếp với bên ngoài, storage cung cấp chỗ lưu dữ liệu, user và permission quyết định ai được truy cập. systemd chạy dịch vụ, firewalld và SELinux bảo vệ dịch vụ đó. Quản lý tài nguyên theo dõi toàn bộ hệ thống. Container được xây từ chính các tính năng của kernel Linux.
Hiểu một khái niệm ở lớp dưới thì học lớp trên nhanh hơn nhiều.
Nguyên tắc 1: "không vào được web" có thể nằm ở mạng, firewall, quyền file, SELinux hoặc dịch vụ đã dừng. Nguyên tắc 4: snapshot hoặc backup trước thay đổi lớn, kiểm tra cấu hình như mount -a trước khi khởi động lại.
Tự đánh giá: các lệnh trong báo cáo em mới hiểu ở mức đọc và giải thích được, chưa thao tác thành thạo, chưa tự xử lý sự cố trong môi trường thật.
-->

---
layout: title-left-card
class: dense
---

<span class="lpc-tag">Định hướng sắp tới</span>

# Định hướng sắp tới

<TitleLine width="260px" />

<p class="!m-0">Thực hành làm chắc nền tảng → Kubernetes → AWS. Song song: Red Hat.</p>

<div class="lpc-mini mt-3"><h3 class="!mb-0">Giai đoạn 1 · Thực hành và củng cố nền tảng</h3></div>

<div class="lpc-mini mt-3"><h3 class="!mb-0">Giai đoạn 2 · Kubernetes</h3></div>

::right::

<div class="flex flex-col gap-3 h-full justify-center">
  <div class="lpc-mini"><h3 class="!mb-0">Giai đoạn 3 · AWS SAA-C03</h3></div>
  <div class="lpc-mini" style="border-top-color: var(--lpc-orange)"><h3 class="!mb-0">Giai đoạn 4 · Red Hat (song song)</h3></div>
</div>

<!--
Giai đoạn 1: thực hành lại toàn bộ báo cáo trên RHEL 10.2 (VM và snapshot, backup, clone; 2 VM kết nối bằng nmcli; LVM; phân quyền nhóm; web với systemd, firewalld, SELinux; giả lập tải; Docker web + database). Ghi chép lỗi, viết script Bash.
Giai đoạn 2: Kubernetes, dựng cụm nhỏ bằng kubeadm trên 3 VM (1 control plane, 2 worker).
Giai đoạn 3: AWS SAA-C03, Well-Architected: bảo mật 30%, phục hồi 26%, hiệu năng 24%, chi phí 20%. Lab trên Free Tier, có cảnh báo chi phí.
Giai đoạn 4: RHCSA (EX200) thi thực hành trên RHEL 10, học thêm Podman, sau đó RHCE (EX294) về Ansible.

Lộ trình đi theo logic nối tiếp: thực hành để làm chắc nền tảng; sau đó Kubernetes, bước tự nhiên sau container; tiếp theo đưa kiến thức lên cloud với AWS; song song đào sâu Linux doanh nghiệp theo chuẩn Red Hat.
Kubernetes tự khởi động lại container khi lỗi, tự mở rộng khi tải tăng, cập nhật phiên bản không gián đoạn.
Nội dung RHCSA trùng với nền tảng trong báo cáo: storage và LVM, user và permission, systemd, firewalld, mạng và SELinux.
-->

---
layout: qa
---

# Q&A

<!--
Em rất mong nhận được góp ý của anh Khoa và các bạn về lộ trình này, đặc biệt là những kỹ năng mà team đang cần, để em ưu tiên thứ tự học cho phù hợp với công việc thực tế.
-->

---
layout: thank-you
image: /images/thanks-main.jpg
image2: /images/thanks-small.jpg
---

<span class="lpc-pill !text-xl !px-8 !py-4 whitespace-nowrap">Cảm ơn anh Khoa và các bạn đã lắng nghe</span>

<TitleLine width="100%" />

<p class="!text-xl text-[var(--lpc-muted)]">Rất mong nhận được góp ý về lộ trình học của em.</p>

<!--
Em cảm ơn anh Khoa và các bạn đã dành thời gian lắng nghe.
-->
