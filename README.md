# BÀI TẬP LỚN PHÁT TRIỂN ỨNG DỤNG TRÊN THIẾT BỊ DI ĐỘNG
# Sinh viên: Đậu Văn Khánh
# MSSV: K225480106099
# Lớp: K58KTP

# VIẾT APP SỬ DỤNG ANDROID STUDIO

## 1. AndroidManifest.xml là gì?
- Khái niệm: File AndroidManifest.xml là file cấu hình trung tâm của ứng dụng Android.
- Nó dùng để mô tả:
  + Tên package.
  + Các Activity.
  + Service.
  + Broadcast Receiver.
  + Quyền truy cập (Permissions).
  + Phiên bản Android hỗ trợ.
  + Các tính năng phần cứng cần sử dụng.
- Ví dụ:
```
<manifest xmlns:android=
    "http://schemas.android.com/apk/res/android">

    <uses-permission
        android:name="android.permission.INTERNET"/>

    <application
        android:label="My App">

        <activity
            android:name=".MainActivity">

            <intent-filter>
                <action
                    android:name=
                    "android.intent.action.MAIN"/>

                <category
                    android:name=
                    "android.intent.category.LAUNCHER"/>
            </intent-filter>

        </activity>

    </application>

</manifest>
```
## 2. Khai báo quyền (Permission)
- Permission là cơ chế bảo mật của Android, giúp kiểm soát việc ứng dụng truy cập các tài nguyên nhạy cảm trên thiết bị.
- Một số quyền phổ biến:
```
| Quyền                 | Chức năng                  |
| --------------------- | -------------------------- |
| INTERNET              | Truy cập Internet, gọi API |
| CAMERA                | Chụp ảnh                   |
| ACCESS_FINE_LOCATION  | Lấy vị trí GPS             |
| READ_EXTERNAL_STORAGE | Đọc dữ liệu bộ nhớ         |
```
- Muốn dùng phải khai báo quyền.
- Ví dụ quyền Internet:
```
<uses-permission
    android:name="android.permission.INTERNET"/>
```
- Cho phép:
  + Gọi API
  + Tải dữ liệu web
  + WebView
- Ví dụ Camera:
```
<uses-permission
    android:name="android.permission.CAMERA"/>
```
- Cho phép:
  + Chụp ảnh
  + Quét QR

## 3. Vòng đời Activity Android
### Activity là một màn hình giao diện trong ứng dụng Android.
### Một Activity có vòng đời như sau:
```
onCreate()
    ↓
onStart()
    ↓
onResume()
(App hoạt động)
    ↓
onPause()
    ↓
onStop()
    ↓
onDestroy()
```
### onCreate()
- Đây là hàm được gọi đầu tiên khi Activity được tạo.
- Android Studio tự sinh hàm này vì đây là nơi:
  + Khởi tạo giao diện.
  + Khởi tạo dữ liệu.
  + Ánh xạ các điều khiển.
- Ví dụ:
```
@Override
protected void onCreate(Bundle savedInstanceState)
{
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
}
```
### Tại sao Android Studio tự sinh onCreate()?
- Khi tạo Project, Android Studio tạo sẵn: onCreate()
- Vì:
  + Activity bắt buộc phải có điểm khởi đầu.
  + Giao diện phải được nạp bằng: setContentView()
- Nếu không có: setContentView() thì màn hình sẽ không hiển thị giao diện.
### onStart()
- Được gọi khi: Activity bắt đầu hiển thị cho người dùng.
### onResume()
- Được gọi khi: Người dùng có thể tương tác với Activity.
- Ví dụ:
  + Bấm nút.
  + Nhập dữ liệu
### onPause()
- Xảy ra khi: Một Activity khác xuất hiện tạm thời.
- Ví dụ:
  + Có cuộc gọi đến.
  + Activity vẫn còn tồn tại nhưng mất quyền tương tác.
### onStop()
- Activity không còn hiển thị.
- Ví dụ: Người dùng chuyển sang ứng dụng khác.
### onDestroy()
- Được gọi trước khi Activity bị hủy.
- Ví dụ:
  + Người dùng thoát ứng dụng.
  + Hệ thống giải phóng bộ nhớ.
## 4. Kiểm tra quyền trong Java
### Android sử dụng cơ chế Permission để bảo vệ dữ liệu người dùng. Trước khi sử dụng Camera, GPS hoặc Bộ nhớ, ứng dụng cần kiểm tra quyền truy cập.
### Kiểm tra quyền
```
if(ContextCompat.checkSelfPermission(
        this,
        Manifest.permission.CAMERA)
        == PackageManager.PERMISSION_GRANTED)
{
}
```
- Ý nghĩa: Kiểm tra xem ứng dụng đã được cấp quyền Camera hay chưa.
### Xin quyền
```
ActivityCompat.requestPermissions(
        this,
        new String[]{
                Manifest.permission.CAMERA
        },
        1);
```
- Ý nghĩa: Hiển thị hộp thoại yêu cầu người dùng cấp quyền Camera.
## 5. Layout XML
- Trong Android, giao diện người dùng được mô tả bằng các file XML nằm trong thư mục:
```
res
 └── layout
```
- Mỗi Activity thường đi kèm một file Layout để định nghĩa các thành phần giao diện như TextView, Button, EditText,...
- Ví dụ:
```
<TextView
    android:id="@+id/txtHello"
    android:text="Hello"/>
```
- Việc sử dụng XML giúp tách biệt giao diện và mã xử lý, thuận tiện cho việc bảo trì và phát triển ứng dụng.
## 6. Resource và cơ chế tham chiếu
- Android khuyến khích không ghi trực tiếp (hardcode) nội dung lên giao diện mà nên lưu trong Resource.
- Ví dụ không nên:
```
android:text="Xin chào"
```
- Nên sử dụng:
```
android:text="@string/hello"
```
- Chuỗi ký tự được lưu trong file:
```
res
 └── values
      └── strings.xml
```
- Ví dụ:
```
<resources>
    <string name="hello">
        Xin chào
    </string>
</resources>
```
- Ngoài String, Android còn hỗ trợ nhiều loại Resource khác:
```
| Loại tài nguyên | Cú pháp        |
| --------------- | -------------- |
| String          | @string/hello  |
| Color           | @color/red     |
| Drawable        | @drawable/logo |
```
- Ưu điểm:
  + Không phải sửa nhiều nơi khi thay đổi nội dung.
  + Dễ dàng tái sử dụng dữ liệu.
  + Hỗ trợ đa ngôn ngữ.
  + Hỗ trợ nhiều giao diện khác nhau.
## 7. Hỗ trợ đa ngôn ngữ và Theme
- Android cho phép tạo nhiều thư mục Resource tương ứng với từng ngôn ngữ.
- Ví dụ: 
```values/```- Tiếng Việt

```values-en/```- Tiếng Anh
- File tiếng Việt:
```
<string name="hello">
    Xin chào
</string>
```
- File tiếng Anh:
```
<string name="hello">
    Hello
</string>
```
- Khi người dùng chuyển ngôn ngữ hệ thống, Android sẽ tự động lựa chọn nội dung phù hợp:
```
Language = English
→ Hello

Language = Vietnamese
→ Xin chào
```
- Tương tự, Android hỗ trợ Dark Mode thông qua các thư mục: values/
- Light Theme: values-night/
- Dark Theme: Hệ điều hành sẽ tự động chuyển đổi giao diện theo thiết lập của người dùng.

## 8. Các Layout chứa đối tượng con
- Layout là thành phần dùng để chứa và sắp xếp các đối tượng giao diện.
- Một Layout phổ biến là LinearLayout.
- Sắp xếp theo chiều dọc:
```
<LinearLayout
    android:orientation="vertical">
</LinearLayout>
```
- Sắp xếp theo chiều ngang:
```
<LinearLayout
    android:orientation="horizontal">
</LinearLayout>
```
- Thuộc tính orientation quyết định cách hiển thị các đối tượng con bên trong Layout.

## 9. Gravity
- Gravity dùng để căn chỉnh vị trí hiển thị của các thành phần giao diện.
- Ví dụ:
```
android:gravity="center"
```
- Giúp căn giữa nội dung bên trong đối tượng hoặc Layout.
- Một số giá trị thường dùng: center, left, right, top, bottom.

## 10. Hiển thị dữ liệu theo Resource
- Để hỗ trợ đa ngôn ngữ, không nên gán trực tiếp chuỗi ký tự trong Java.
- Không nên:
```
txt.setText("Xin chào");
```
- Nên sử dụng:
```
txt.setText(
    getString(R.string.hello)
);
```
- Android sẽ tự động lấy nội dung tương ứng với ngôn ngữ hiện tại của thiết bị.

## 11. Xử lý sự kiện (Event)
- Event là các hành động của người dùng tác động lên ứng dụng như:
  + Click Button.
  + Click TextView.
  + Touch.
  + Long Click
### Cách 1: Xử lý sự kiện trong Java
- Layout:
```
<Button
    android:id="@+id/btnOK"/>
```
- Java:
```
Button btn =
findViewById(R.id.btnOK);

btn.setOnClickListener(v ->
{
    Toast.makeText(
            this,
            "Hello",
            Toast.LENGTH_SHORT)
            .show();
});
```
- Đây là cách được sử dụng phổ biến nhất vì dễ quản lý và linh hoạt.

### Cách 2: Sử dụng onClick trong XML
- Layout:
```
<Button
    android:onClick="xuLyNut"/>
```
- Java:
```
public void xuLyNut(View v)
{
    Toast.makeText(
            this,
            "Hello",
            Toast.LENGTH_SHORT)
            .show();
}
```
- Cách này đơn giản nhưng ít được sử dụng trong các dự án lớn.

## 12. Thư mục Assets trong Android
### Khái niệm: Assets là thư mục đặc biệt dùng để lưu các tập tin dữ liệu đi kèm ứng dụng.
### Vị trí:
```
app
 └── src
      └── main
           └── assets
```
### Assets có thể chứa:
- File TXT
- File JSON
- File HTML
- Hình ảnh
- Âm thanh
- Video
- PDF
### Vai trò của Assets:
- Khi ứng dụng được biên dịch (Build APK), toàn bộ dữ liệu trong thư mục Assets sẽ được đóng gói cùng ứng dụng.
```
Assets
    ↓
Build APK
    ↓
Đi theo ứng dụng
```
- Do đó dữ liệu luôn có sẵn trên thiết bị kể cả khi không có Internet.
### Truy cập dữ liệu trong Assets:
- Android sử dụng AssetManager để đọc dữ liệu từ Assets.
- Ví dụ:
```
InputStream is =
getAssets().open("data.txt");
```
- Nếu file nằm trong thư mục con:
```
assets
 └── guide
      └── android.txt
```
- Truy cập:
```
InputStream is =
getAssets().open("guide/android.txt");
```
### Lợi ích của Assets:
- Hoạt động được khi offline.
- Dữ liệu luôn có sẵn trong ứng dụng.
- Tốc độ truy xuất nhanh.
- Không phụ thuộc Internet hoặc máy chủ.
- Dễ triển khai và sử dụng.
### Ứng dụng thực tế:
- Assets thường được sử dụng trong:
  + App học tập.
  + App hướng dẫn sử dụng.
  + App tra cứu từ điển.
  + App đọc tài liệu offline.

# Viết app sử dụng Android Studio
## 1. Tạo App 1 - Sử dụng cơ chế dữ liệu chuẩn bị trước trong Assets
## Cấu trúc thư mục
```
app
│
├── assets
│   └── vietnam_heritages.json
│
├── java
│   ├── MainActivity.java
│   ├── Heritage.java
│   └── HeritageAdapter.java
│
└── res
    └── layout
        ├── activity_main.xml
        └── item_heritage.xml
```
## Tạo project mới trong Android Studio.
- Trong Android Studio chọn File -> New -> chọn New Project -> Empty Views Activity -> Cấu hình project -> Finish
<img width="1129" height="809" alt="image" src="https://github.com/user-attachments/assets/8c82f7af-00ca-4365-8589-1aebe8045026" />

## Tạo thư mục Assets
- Nhìn bên trái mở app -> mở src -> main
- Chuột phải main -> New -> Diretory -> assets
<img width="1920" height="1200" alt="13" src="https://github.com/user-attachments/assets/be63901b-382a-4754-a3c1-02631f733a39" />

- Kết quả:
<img width="626" height="409" alt="image" src="https://github.com/user-attachments/assets/1c99f077-acf5-4d28-a899-dfc5deb8c2fe" />

- Tạo file JSON
  + Chuột phải vào assets -> New -> File -> Đặt tên: vietnam_heritages.json
<img width="1920" height="1200" alt="13" src="https://github.com/user-attachments/assets/732405f0-566b-4036-a7e7-63ab386790ac" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d84f5e55-33b3-4341-b7f3-6b7668a7880a" />

## Thiết kế giao diện chính
- Mở res -> layout -> activity_main.xml
<img width="619" height="758" alt="image" src="https://github.com/user-attachments/assets/46c6c221-38b4-4637-931a-3f95a30d91dd" />

- Xóa toàn bộ nội dung -> thêm nội dung:
```
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="12dp">

    <TextView
        android:id="@+id/txtHeader"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Khám Phá Di Sản Việt Nam"
        android:textSize="26sp"
        android:textStyle="bold"
        android:gravity="center"
        android:padding="16dp"/>

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</LinearLayout>
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b3dc5697-08b0-4631-95f6-e94f99e4329a" />

- Ý nghĩa:
  + RecyclerView dùng để hiển thị danh sách các di sản.
  + Mỗi di sản sẽ được hiển thị dưới dạng Card.

## Tạo class Heritage
- Chuột phải package: java -> com.example.androidguideapp
- Chuột phải vào com.example.androidguideapp -> new -> Java Class
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/14dabb28-a586-4d0d-b53f-bf961292a4ca" />

- Mục đích:
  + Lưu dữ liệu của một di sản.
  + Mỗi di sản gồm: tiêu đề, danh mục, vị trí, nội dung.
  
## Tạo file item_heritage.xml
- Mở res -> chuột phải vào layout -> new -> Layout Resource File -> Đặt tên: item_heritage.xml
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/2b395f05-4e58-4737-8ea5-b83f7461f6d4" />

- Mục đích:
  + Hiển thị một di sản.
  + Bao gồm: Tên di sản, loại di sản, địa điểm, nút xem chi tiết.

## Tạo class HeritageAdapter
- Mở java -> chuột phải com.example.androidguideapp -> new -> Java Class
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b0bab912-6ed7-4365-a19c-eaa0e8702413" />

- Mục đích:
  + Đọc dữ liệu từ danh sách Heritage.
  + Hiển thị dữ liệu lên RecyclerView.
  + Xử lý nút Xem chi tiết.

## Sửa file MainActivity.java
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/9f6834b2-0f42-44d1-96bb-c33098afa4a6" />

## Tạo máy ảo Android (AVD)
- Trên thanh menu chọn Tools -> Device Manager
- Cửa sổ Device Manager sẽ mở bên phải.
<img width="1920" height="1200" alt="14" src="https://github.com/user-attachments/assets/c2143402-02b1-44f8-a1d9-1c11153f9403" />

- Bấm + Create Device -> chọn Phone -> Pixel 6 -> Next
<img width="1126" height="851" alt="image" src="https://github.com/user-attachments/assets/179b4229-cefc-4e12-82dd-98e1362f8fb1" />

- Chọn API 35 -> Finish
<img width="1100" height="879" alt="image" src="https://github.com/user-attachments/assets/ccbe4cef-b750-4ca4-bb82-6a27e0ef3fbc" />

## Kết quả
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/f553cf46-efc8-423b-b896-4cc1c80a1f08" />

- Nhấn vào xem chi tiết sẽ hiện nội dung đầy đủ
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/a453be18-7f38-4594-ba1f-222d76438977" />

## 2. Tạo App 2 - Tạo app tương đương với Mit App inventor
### Yêu cầu:
- Ứng dụng gồm 3 Activity
```
| Activity  | Chức năng                                      |
| --------- | ---------------------------------------------- |
| Activity1 | About + nút chuyển sang Activity2 và Activity3 |
| Activity2 | Giải toán đơn giản + gửi kết quả lên API       |
| Activity3 | WebView mở trang web theo MSSV                 |
```
### Tạo project mới:
- Trong Android Studio chọn File -> New -> chọn New Project -> Empty Views Activity -> Cấu hình project -> Finish
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/f0929a93-e839-4001-8882-cc5bda6f575e" />

### Tạo thêm activity 2, 3
- Mặc định có: MainActivity -> dùng làm Activity1.
- Tạo Activity2:
  + Trong cột Project: truy cập app -> kotlin + java -> com.example.app2
  + Chuột phải vào package com.example.app2mitinventor → New → Activity → Empty Views Activity.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/22e17778-242a-420a-89bc-cf4dea688953" />

- Tạo các Activity sau:
  + AboutActivity
  + MathActivity
  + WebActivity
<img width="693" height="537" alt="image" src="https://github.com/user-attachments/assets/8be8277f-a423-4325-b398-f5eab64db6a9" />

```
| Activity      | File Java          | Layout             | Mô tả                             |
| ------------- | ------------------ | ------------------ | --------------------------------- |
| AboutActivity | AboutActivity.java | activity_about.xml | Màn hình giới thiệu và điều hướng |
| MathActivity  | MathActivity.java  | activity_math.xml  | Màn hình giải toán và gọi API     |
| WebActivity   | WebActivity.java   | activity_web.xml   | Màn hình WebView hiển thị website |
```

### Cấu hình quyền Internet
- Mở file: app → manifests → AndroidManifest.xml
- Thêm quyền truy cập Internet:
```
<uses-permission android:name="android.permission.INTERNET"/>
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4e04b5e6-baf3-4aab-81b2-d4815accccaf" />

### Thiết kế màn hình About (Activity 1)
- Giao diện:
  + Mở file: res → layout → activity_about.xml
  + Thiết kế giao diện gồm:
    + Tên ứng dụng.
    + MSSV, họ tên sinh viên, môn học.
    + Nút mở màn hình Giải toán.
    + Nút mở màn hình WebView.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/fa7952c4-3d6b-4689-a5ec-b2990458a192" />

- Xử lý điều hướng:
  + Mở file: AboutActivity.java
  + Trong AboutActivity.java, sử dụng Intent để chuyển sang: MathActivity và WebActivity.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/da4c5a59-a445-4b40-b688-42e3936403eb" />

### Xây dựng màn hình Giải toán và Gọi API (Activity 2)
-Giao diện:
  + Mở file: res → layout → activity_math.xml
  + Thiết kế giao diện gồm:
    + 3 ô nhập dữ liệu a, b, c.
    + Nút "GIẢI TOÁN".
    + TextView hiển thị kết quả.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/15563e9f-88a2-4074-a698-48f74a1f6091" />

- Xử lý giải toán và gửi API:
  + Mở file: MathActivity.java
  + Trong MathActivity.java:
    + Xử lý giải phương trình.
    + Tạo dữ liệu JSON theo cấu trúc yêu cầu.
    + Gửi dữ liệu đến API: https://k58kmt.tdh.io.vn/api
    + Nhận phản hồi từ Server và hiển thị kết quả.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/512e4e44-f0ba-4469-80da-da61dded0d71" />

### Xây dựng màn hình WebView
- Giao diện:
  + Mở file: res → layout → activity_web.xml
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/46fa353e-d6bc-4dec-88c2-963b26072539" />

- Xử lý WebView:
  + Mở file: WebActivity.java
  + Trong WebActivity.java:
    + Bật JavaScript.
    + Tạo URL: https://k58kmt.tdh.io.vn?masv=K225480106099
    + Sử dụng loadUrl() để hiển thị trang web trong ứng dụng.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/89a3f721-f43d-4746-9c4d-3915393318cd" />

### Kiểm tra chương trình
### Màn hình About
- Hiển thị đúng thông tin sinh viên.
- Các nút điều hướng hoạt động chính xác.

### Màn hình Giải toán
Nhập dữ liệu và giải phương trình thành công.
Gửi dữ liệu lên API và nhận phản hồi từ Server.
Màn hình WebView
Tải thành công trang web.
Hiển thị đúng mã sinh viên trên URL.
