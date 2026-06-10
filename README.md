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
- Ví dụ: App hướng dẫn học Android
```
assets
 ├── activity.txt
 ├── intent.txt
 ├── service.txt
 └── broadcast.txt
```
- Ứng dụng đọc dữ liệu từ Assets và hiển thị cho người dùng mà không cần kết nối Internet.

# Viết app sử dụng Android Studio
## 1. Tạo App 1
## Cấu trúc thư mục
```
app
│
├── assets
│   └── android_lessons.json
│
├── java
│   ├── MainActivity.java
│   ├── Lesson.java
│   └── LessonAdapter.java
│
└── res
    └── layout
        ├── activity_main.xml
        └── item_lesson.xml
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
  + Chuột phải vào assets -> New -> File -> Đặt tên: android_lessons.json
<img width="1920" height="1200" alt="13" src="https://github.com/user-attachments/assets/732405f0-566b-4036-a7e7-63ab386790ac" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/85923844-63ad-4106-bd7c-e5152642ddfa" />

## Thiết kế giao diện chính
- Mở res -> layout -> activity_main.xml
<img width="619" height="758" alt="image" src="https://github.com/user-attachments/assets/46c6c221-38b4-4637-931a-3f95a30d91dd" />

- Xóa toàn bộ nội dung -> thêm nội dung:
```
<?xml version="1.0" encoding="utf-8"?>

<androidx.recyclerview.widget.RecyclerView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/recyclerView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4ab69254-97f8-46d6-82bb-72c625cab294" />

- Ý nghĩa: RecyclerView sẽ hiển thị danh sách bài học.

## Tạo class lesson
- Chuột phải package: java -> com.example.androidguideapp
- Chuột phải vào com.example.androidguideapp -> new -> Java Class
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/27b9b5b7-2d7d-4aa4-b805-74499c1d2dbb" />

- Mục đích:
  + Tạo đối tượng lưu dữ liệu của một bài học.
  + Mỗi bài học gồm: title (tiêu đề) và content (nội dung).
  + Dữ liệu được đọc từ file JSON trong Assets và lưu vào đối tượng Lesson.
  
## Tạo file item_lesson.xml
- Mở res -> chuột phải vào layout -> new -> Layout Resource File -> Đặt tên
<img width="574" height="237" alt="image" src="https://github.com/user-attachments/assets/e676bb03-ca14-466b-abc5-73f7667720ba" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/8cc1f561-9f11-4ee3-b4d2-a4ba987d7090" />

- Mục đích:
  + Thiết kế giao diện hiển thị cho một bài học.
  + Bao gồm:
    + txtTitle: hiển thị tiêu đề.
    + txtContent: hiển thị nội dung
  + Layout này sẽ được RecyclerView sử dụng để hiển thị từng dòng dữ liệu.

## Tạo class LessonAdapter
- Mở java -> chuột phải com.example.androidguideapp -> new -> Java Class
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d181eafc-c13e-4682-8e55-c0d713bfe8c2" />

- Mục đích:
  + Là cầu nối giữa dữ liệu (Lesson) và giao diện (item_lesson.xml).
  + Nhận danh sách bài học từ file JSON.
  + Đưa dữ liệu vào các TextView để hiển thị trên RecyclerView.

## Sửa file MainActivity.java
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/ccf7ff53-8b35-4be4-9044-63dfeefac460" />

## Tạo máy ảo Android (AVD)
- Trên thanh menu chọn Tools -> Device Manager
- Cửa sổ Device Manager sẽ mở bên phải.
<img width="1920" height="1200" alt="13" src="https://github.com/user-attachments/assets/cb1c4be7-892c-45f6-8544-f1bdc4d43a6d" />

- Bấm + Create Device -> chọn Phone -> Pixel 6 -> Next
<img width="1126" height="851" alt="image" src="https://github.com/user-attachments/assets/179b4229-cefc-4e12-82dd-98e1362f8fb1" />

- Chọn API 35 -> Finish
<img width="1920" height="1200" alt="13" src="https://github.com/user-attachments/assets/c6fc2eb2-1014-498d-a533-66c4fea20a70" />

## Kết quả
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/8a910480-565e-42cf-ad59-ed8b08a9c257" />

- Nhấn vào xem chi tiết sẽ hiện nội dung đầy đủ
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/1064fcf5-6991-4be6-b9a2-a0b75839fe3c" />

## 2. App 2
