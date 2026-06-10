# BÀI TẬP LỚN PHÁT TRIỂN ỨNG DỤNG TRÊN THIẾT BỊ DI ĐỘNG
# Sinh viên: Đậu Văn Khánh
# MSSV: K225480106099
# Lớp: K58KTP
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
