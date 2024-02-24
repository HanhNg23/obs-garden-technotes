---
{"dg-publish":true,"dg-permalink":"FPT-Prn211-Review","permalink":"/FPT-Prn211-Review/"}
---


# 🗒️ DOT NET PRN211

---

## TIỀN BÀI HỌC - REVIEW KIẾN THỨC VỀ JAVA
#### 1. OOP, Class, Object, SOLID
> Java - ngôn ngữ lập trình hướng đối tượng và hoạt động dựa trên 4 tính chất của OOP và 5 nguyên lý theo SOLID
- **OOP**
	- 4 đặc tính của OOP theo thứ tự: 
		- **Abstraction** 
			- Trừu tượng hóa đối tượng bên ngoài thành một object gói gọn 
			<span style="color:#555555">Trừu tượng hóa object từ thế giới thực sang thế giới máy. --> Lấy ra những info quan trọng, đặc trưng từ thế giới thực --> Biểu diễn lại đơn giản hơn nhưng vẫn giữ được bản chất.</span>
	
			- ![](https://cdn-images.visual-paradigm.com/guide/uml/uml-class-diagram-tutorial/01-uml-base-class-and-object-explained.png)
			
		- **Encapsulation** 
			- Quy định quyền truy cập vào đối tượng, giới hạn khả năng bị truy cập bởi các đối tượng khác với mục đích bảo vệ đối tượng không bị truy cập từ code bên ngoài vào thay thế các giá trị của thuộc tính trực tiếp.
		    > Từ khóa quy định phạm vi truy cập
		    > - từ khóa [modifier](https://usemynotes.com/wp-content/uploads/2021/02/what-are-access-specifiers-in-java.jpg): public, protected, private, default
		    > - từ khóa getter, setter 
		    
		    ![](https://i.imgur.com/tWWjVp8.png)

		- **Inheritance** 
			- Class con kế thừa class cha - class con thừa hưởng lại những thuộc tính, phương thức từ lớp cha --> nhằm mục đích sử dụng lại - tái sử dụng code.
			
				- ![](https://i.imgur.com/RSjinBK.png)

		- **Polymorphism** 
			Thể hiện qua 2 hình thức sau:
			- Có nhiều lớp con kế thừa chung lớp cha nhưng các lớp con khác nhau có những tính chất thể hiện khác nhau -> đa hình 
			  > từ khóa: overriding, overloading, interface
			- Những tác vụ/phương thức trong cùng một đối tượng thể hiện nhiều cách khác nhau -> đa hình
			  > vd: trong cùng một class, có nhiều phương thức tác vụ thực cùng kiểu dữ liệu trả về và giống nhau về nhưng có thể khác nhau về số kiểu tham số || kiểu dữ liệu. 
			- Mục đích tạo ra nhiều biến thể cùng loại
- **Class** [See](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/uml-class-diagram-tutorial/#uml-class-diagram-what-is-a-class)
	- Là khuôn mẫu / blueprint / khung xương - vai trò: defines the attributes and behaviors of OBJECTs
- **Object** 
	- Là các thực thể - các instances của một class, là da thịt được đắp lên khung xương - vai trò: representing a specific occurence or realization of the class.
	>vd: Ta có class car - khung sườn bao gồm: màu xe, thương hiệu xe, có khả năng chạy xe
	>--> instance của class car ta lấy Toyota car làm vd
	>Toyota car có màu xe: đen, thương hiệu: Toyota, có khả năng chạy xe. 
	>--> Toyota car một thực thể có da có thịt đầy đủ 
- **[SOLID](https://gpcoder.com/4200-cac-nguyen-ly-thiet-ke-huong-doi-tuong/)** - *nguyên lý design code the hướng đối tượng*

#### 2. Khái niệm Write One Run Anywhere (WORA)
  - (Trước kia) <span style="color:#91819c">Platform-dependent</span> - bộ cấu hình máy tính gồm hardware (cụ thể CPU) + software --> tạo môi trường cho chương trình mãy tính thực thi trên OS --> viết chương trình cho OS nào chỉ chạy cho OS đó --> gây bất cập khi muốn chương trình được chạy trên nhiều hệ điều hành khác nhau
   ==> <span style="color:#91819c">Platform-independent</span> ra đời - thực thi code của một chương trình trên bất kì hệ điều hành OS nào với điều kiện phải cài đặt môi trường ảo riêng để chạy trên từng loại OS cụ thể - runtime environment.
  > Thực thi code Java trên đa nền tảng sẽ thông qua khái niệm Platform-independent - code của Java sẽ được chạy trên môi trường ảo là **JVM -Java Virtual Machine** - Platform-dependent, JVM tùy loại OS sẽ được thiết kế riêng để cài đặt. Vd như Mac OS X, Window, Linux thì sẽ có JVM riêng cho mỗi OS trên. ([See](https://www.geeksforgeeks.org/java-platform-independent/))

## I. .NET, .NET FRAMEWORK, .NET CORE 
   🫱 [See](https://learn.microsoft.com/en-us/dotnet/core/introduction)
- .NET là một cross platform - chạy đa nền, app chạy không phụ thuộc OS - write once run anywhere (WORA)
- .NET là nền tảng môi trường, bộ thư viện, cung cấp toàn bộ tài nguyên cho việc chạy app .NET, C#, VB.NET, C++.NET
- .NET có thể được viết bằng ngôn ngữ lập trình C#, F#, Visual Basic. 
   > Trong chương trình học của môn PRN211 -> viết .NET bằng C# trên visual-studio
- Hệ sinh thái .NET gồm có (theo thứ tự thời gian)
	- .NET Framework -- work as JDK idea of Java -- Hiện tại chỉ work với window
	- Mono -- triển khai từ .NET Framework nhưng thiết kế để trở thành cross-platform (Một framework hoạt động trên nhiều hệ điều hành khác nhau)
	- .NET (Core) -- .NET CROSS PLATFORM - là bản kế thừa open source của .NET Framework nhưng được thiết kế để hoạt động như một cross-platform. Used for Linux, macOS, and Windows apps. 
> [.Net History](https://learn.microsoft.com/en-us/dotnet/core/introduction#net-history)
- Muốn .NET chạy được trên đa nền tảng, phải cài đặt 2 Binary distributions sau:
	- .NET SDK - bộ tool, thư viện và runtimes cho việc phát triển, xây dựng và kiểm tra apps .NET
	> (tương tự JDK - bộ công cụ cho nhà lập trình viên ứng dụng java)
	- .NET Runtimes - bộ runtimes và thư viện hỗ trợ running apps 
	
- Trình biên dịch trong .NET - Compilation - [See](https://learn.microsoft.com/en-us/dotnet/core/introduction#compilation)
	- Code được viết ra cho ứng dụng .NET được biên dịch sang ngôn ngữ trung gian IL - Intermediate Language (tương tự byte code - giống style JAVA).
	- IL - a compact code format - một định dạng mã nhỏ, tiêu tốn ít dung lượng, code format này có thể sử dụng trên trên nhiều OS và kiến trúc
	- Để biên dịch IL sang ngôn ngữ máy để được thực thi chương trình trên CPU thì 2 mô hình biên dịch sẽ được dùng là JIT(Just-In-Time) và AOT(Ahead-Of-Time) - và quá trình chạy code/thực thi diễn ra trong môi trường ảo - runtime environment có tên gọi là CLR - Common Language Runtime (a virtual machine - of Microsoft .NET framework that manages the execution of .NET programs)
	- ![](https://i.imgur.com/WjdN2zg.png)
	> [link](https://www.baeldung.com/cs/runtime-vs-compile-time)


## II. CẤU TRÚC DỰ ÁN C# - SOLUTION, PROJECT, FILE MÃ NGUỒN
1. <span style="color:#00b0f0">**Cấu trúc solution/project của C#**</span>
	- C# quản lý mã nguồn theo cấu trúc cây gần giống với cấu trúc thư mục và bao gồm 2 cấp độ cơ bản: Project và Solution
2. <span style="color:#00b0f0">**Project trong C#**</span>
	- Project là cấp độ quản lý mã nguồn quan trọng vì mỗi project sau khi biên dịch sẽ tạo ra một chương trình.
	- Mỗi project mặc định đều chứa:
		- Các file mã nguồn: là các file văn bản có phần mở rộng .cs (C Sharp)
		- Các file cấu hình của chương trình: là file xml có phần mở rộng .config
		- Các thư viện được tham chiếu tới - References: là danh sách các file thư viện chuẩn của .NET framework, hoặc thư viện từ các hãng thứ 3, hoặc từ chính các project khác, chuyên chứa các class được sử dụng bởi class trong project này
		- Các thuộc tính - Properties: bao gồm nhiều loại thông tin khác nhau quyết định những tính chất quan trọng của project, như phiên bản của nào .NET Framework được sử dụng, loại chương trình mà dự án này sẽ được dịch thành là loại nào, các tài nguyên được sử dụng trong project gồm gì, cấu hình của ứng dụng, ...
3. <span style="color:#00b0f0">**Solution trong C#**</span>
	- Solution là cấp độ quản lý mã nguồn cao nhất trong C# cho phép quản lý tập trung nhiều project
	- Mỗi Solution trong C# có thể chứa nhiều project. 
4. <span style="color:#00b0f0">**Cấu trúc file/thư mục của C# project**</span>
	- Tên thư mục được đặt tên theo "Solution Name" do người dùng đặt tên tại phần tạo project.
	- Mỗi project được tạo ra sẽ đặt trong một thư mực con của thư mục solution ở trên và có cùng tên với "project name" do người dùng đặt
	- Trong mỗi thư mục project là các file con
	- Các File cấu hình của solution được lưu trong file có phần đuôi là .sln
		- ![](https://tuhocict.com/wp-content/uploads/2019/01/3-solution-folder.png)
	- Thông tin cấu hình của mỗi project được lưu trong file có tên trùng tên dự án và có phần đuôi là .csproj
		- ![](https://tuhocict.com/wp-content/uploads/2019/01/3-project-folder.png)
5. <span style="color:#00b0f0">**Thư mục bin**</span>
	- Sau khi biên dịch project thành công, trong thư mục của nó sẽ xuất hiện một thư mục con có tên là bin 
	- Biên dịch ở chế độ debug, thư mục bin sẽ xuất hiện thư mục con "Debug" Các file chương trình sau khi biên dịch ở chế dộ này xong sẽ sẽ xuất hiện trong thư mục "Debug." có đường chỉ dẫn chung **"{tên solution}\{tên project}\bin\{Debug}"**
6. <span style="color:#00b0f0">**NET Assembly**</span>
	- Nói rằng mỗi project sau khi biên dịch xong sẽ thành một chương trình --> cách nói “chương trình” không hoàn toàn phù hợp đối với .NET framework. 
	- --> Trong NET framework, mỗi project sau khi biên dịch đều trở thành một file chứa bytecode CIL - File mã CIL này được gọi là *Assembly*. 
	- .NET framework phân biệt hai loại assembly: 
		- một loại có thể tự nạp vào CLI và thực thi; 
		- một loại không thể tự mình nạp vào CLI mà cần phải có một assembly thuộc loại thứ nhất gọi, hoặc được một tiến trình khác gọi. 
		- --> Loại assembly thứ nhất được lưu trong file có phần mở rộng .exe, tương tự như các file chương trình thực thi khác trong Windows. 
		- --> Loại assembly thứ hai được lưu trong các file có phần mở rộng .dll (Dynamic Link Library), tương tự như các file thư viện của Windows. 
		- ----> ***Việc biên dịch ra .exe hay .dll phụ thuộc vào loại project.***
## III. CODING CONVENTION - QUY ƯỚC ĐẶT TÊN TRONG DỰ ÁN
#### Ngoài Class
1. TÊN SOLUTION  
	- Pascal Case - chữ hoa từng đầu từ, danh từ
	- Template: <span style="color:#0070c0">[TênCôngTy.TênSolution]</span>
	- Template: <span style="color:#0070c0">[TênThươngHiệu.TênSolution]</span>
	> - Tên công ty/thương hiệu vd như Microsoft. ;  Oracle., ...
	> - Tên solution - tên của bài toán lớn cần giải quyết = tên dự án: Fap, StudentManagement,...
	> ví dụ:  FPT.Fap, Giaolang.Fap,..
	
2. TÊN PROJECT 
	- Pascal Case, chữ hoa từng đầu từ, danh từ, chứa tên Solution 
	  Template 
	- <span style="color:#0070c0">[TenCongTy.TenSolution.TenProject1]</span>
	- <span style="color:#0070c0">[TenCongTy.TenSolution.TenProject2]</span>
	- <span style="color:#0070c0">[TenCongTy.TenSolution.TenProject3]</span>
  >  ví dụ
  >  Giaolang.Fap.Studen ->  mỗi project trong dự án FAP sẽ gồm tên Solution 'Giaolang.Fap' tập hợp các class có liên quan đến từng nhóm chức năng 
  >  Giaolang.Fap.Lecturer
  >  Giaolang.Swp391.Authen 
  >  Giaolang.Swp391.Notification
	
3. TÊN NAMESPACE 
	- Dùng Pascal Case - chữ hoa đầu từ, danh từ + chứa tên Solution 
	- Namespace là không gian tên, package, tên gọi gôm chung các class vào 1 cụm logic nào đó (tương đương package trong java) đưa vào một ngôi nhà tên Namespace.
	> -> Cho phép các class được trùng tên khác nhau về package/namespace
	> -> Namespace dùng để chia khu vực

4. TÊN CLASS 
	- Pascal Case, Chữ hoa từng đầu từ, danh từ, thược về namespace, DÙNG DANH TỪ SỐ ÍT !!
   > ví dụ: Student, Lecturer, Animal, Utility, String, ...
    > ví dụ: internal class Student{}  
    >       *internal - là một truong những từ khóa của .NET modifier(internal, public, private, protected, protected internal) - quy định phạm vi truy cập, liên quan đến quyền truy xuất thuộc tính của đối tượng.*
    >        *Trong class, nếu các thuộc tính khai báo trong class không định nghĩa quyền truy cập modifier(private, public,..) thì mặc định các thuộc tính đấy là private.*

#### Trong Class
5. TÊN HÀM (method): 
	- VERB + OBJECT - Pascal Case, chữ hoa từng đầu từ, có động từ đứng đầu
	> Ví dụ: Print(), ToString(), Parse(), Compare(), Equals() ( giống và khác JAVA)	
6. TÊN BIẾN LƯU ĐẶC ĐIỂM CỦA OBJECT - <span style="color:#91819c">INSTANCE VARIABLE, C# gọi LÀ DATA FIELD, BACKED-FIELD</span> 
	- Camel Case cho Danh từ, dùng "" đứng tên biến nếu là biến PRIVATE(chỉ được truy cập trong class)
	
   > Biến lưu đặc điểm của object -> là các biến/field được khai báo trong class, nằm ngoài các hàm(method), block, constructor - chúng được gọi là biến toàn cục của một class(instance variable) - được khởi tạo khi một instance/object của class được tạo và chết đi khi instance/object đã bị hủy.
    > vd: private string id; private string _name;
7. TÊN BIẾN CỤC BỘ - <span style="color:#91819c">LOCAL VARIABLE - BIẾN KHAI BÁO TRONG HÀM||TRÊN THAM SỐ HÀM||BLOCK||CONSTRUCTOR</span> 
	- Dùng Camel Case cho Danh từ
   > Biến cục bộ của hàm||block||constructor được tạo ra khi có lời gọi đến hàm||block||constructor - một khi được tạo ra thì chỉ được truy cập nội bộ trong hàm||block||constructor - chết đi sau khi lời gọi đến chúng thực thi xong.
    > Ví dụ: float pi; int job, string homePhone, setCellPhone(string cellPhone),...
8. TÊN HẰNG SỐ: <span style="color:#91819c">BIẾN MÀ KO CHO PHÉP THAY ĐỔI VALUE, PHẢI GÁN VALUE NGAY KHI ĐƯỢC KHAI BÁO</span> - BIẾN THUỘC CLASS 
	- DÙNG PASCAL CASE 
	- NẾU KHAI BÁO HẰNG Ở MỨC CLASS (khai báo dưới tên class không khai báo hằng trong hàm) -> MẶC NHIÊN C# COI NÓ LÀ BIẾN STATIC MÀ KHÔNG CẦN DÙNG KHÓA STASTIC  
    > Ví dụ: public const double Pi = 3.14; == public static const double Pi = 3.14;
	- > *VỚI JAVA TÊN HẰNG SỐ LÀ CHỮ HOA TOÀN TẬP, VÀ CÓ **SHIFT_GẠCH_PHÂN_CÁCH_CÁC** TỪ KHÁC NHAU.*
9. TÊN DELEGATE: 
	- Đặt tên cho DELEGATE là Danh từ mang ý nghĩa đại diện cho việc xử lý hành động của các hàm mà tên gọi sẽ trỏ đến.
   	- Vì Delegate thường dính dáng đến xử lý các event sự kiện trong lập trình hướng sự kiện.
   	> ví dụ : các nút nhấn trên màn hình - GUI APP
	--> Nên delegate thường có SUFFIX - HẬU TỐ đuôi là <span style="color:#91819c">Handler</span>. Nếu dính đến CALLBACK - HẬU TỐ đuôi là <span style="color:#91819c">Callback</span>
	- Nếu chung chung thì nó có HẬU TỐ <span style="color:#91819c">Delegate</span> = "Em đại diện cho một nhóm hàm". "Khi nào cần dùng hàm cụ thể nào trong nhóm, hãy bảo em trỏ tới hàm đó bằng cách bảo em gọi tên hàm đó nhé !"
	- BIẾN THUỘC VỀ DELEGATE - ĐẠI DIỆN CHO HÀM + DÙNG ĐỂ GỌI HÀM --> TÊN NÊN ĐẶT LÀ : VERB + OBJECT, VERB PHRASE - Động từ chính
	> ví dụ: Danh tử đại diện cho một nhóm các hàm MyDelegate. 
	>        Biến delegate dùng gọi hàm cụ thể, EatDrinkDelegate eat, EatDrinkDelegate drink
10. Tên cho các thành phần của Window Forms
    <span style="color:#555555">Component (nút nhấn, checkbox, ô hội thoại - dialog...) trên Window Forms</span> 
	 - Button -> <span style="color:#659532">btn[tên hành động]</span> vd btnExit
	 - TextBox -> ô nhập, <span style="color:#659532">txt[tên thông tin cần lấy]</span> vd txtName, txtPassword 
	 - DataGridView (lưới/table) -> <span style="color:#659532">grd[tên đối tượng]</span> vd grdStudent
	 - RadioButton(nút vặn) -> <span style="color:#659532">rad[tên đối tượng]</span> vd radColor
	 - CheckBox (tích chọn) -> <span style="color:#659532">chk[tên đối tượng cần yes no]</span> vd chkAgree
	 - ComboBox(hộp xổ) -> <span style="color:#659532">cmb[tên đại diện tập hợp nhóm]</span> vd cmbColor
	 - Form(màn hình, cửa sổ, class) -> 
	 - <span style="color:#659532">frm[tên động tử chỉ mục đích của Form][tên đôi tượng]</span> 
	 - <span style="color:#659532">[tên động từ chỉ mục đích của Form][tên đôi tượng]Form</span>
	   > vd frmAddStudent, AddStudentForm
- > [!Tip] Tham khảo về phân loại biến
  > [Variables in C# - Category](https://www.educba.com/variables-in-c-sharp/)
   > [Variable categories](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/variables#92-variable-categories)



## IV. KIỂU DỮ LIỆU - DATA TYPE (giống và khác JAVA)
1. PHÂN LOẠI THEO CÁC HÀNG GỐC VÀ HÀNG ĐỘ 
   *(pre-defined - primitive, user-defined - non-primitive)* 
	- |             Java             |                  C#                   |
	  |:----------------------------:|:-------------------------------------:|
	  |   byte, short, int, long,    |     byte, short, int, long, float     |
	  | float, char, boolean, double | , char, bool, double,  string, object |
	
	- --> *HÀNG GỐC, NHỮNG KIỂU DATA CÓ SẴN, TỪ ĐÓ GIÚP TA TẠO RA CÁC KIỂU PHỨC TẠP HƠN - GỌI LÀ USER - DEFINED: Class, interface, struct, enum, delegate*
2. PHÂN LOẠI THEO CÁCH LƯU TRỮ TRONG RAM !!!
	- JAVA CHIA LÀM 2 LOẠI DATA TYPE (biến primitive - biến nguyên thủy và biến object - biến tham chiếu)
	  - | PRIMITIVE | OBJECT |
		| --------- | ------ | 
		| --------- | ------ | 
		| byte      | object |  
		| short     | class  |                                      
		| int       |        |                                      
		| long      |        |                                      
		| float     |        |                                      
		| char      |        |                                      
		| boolean   |        |                                      
		| double    |        |                                      
		|           |        |                                      
		|           |        |                                      
	- ![](https://i.imgur.com/yFLBUvz.png) 
	- TRONG C# phân thành 2 loại là 
		- BIẾN VALUE - TYPE (java thì là primitive) CHỈ DÙNG 1 VÙNG RAM ĐỂ LƯU GIÁ TRỊ. VÙNG RAM NÀY CHIẾM BAO NHIÊU BYTE TÙY THUỘC LOẠI DATA TYPE

	    ví dụ: 
		* > byte : 1 byte
		* > short: 2 byte
		* > int: 4 byte
		* > float: 4 byte
		* > long: 8 byte
		* > double: 8 byte
		* > char: 2 byte (Unicode)
	- BIẾN REFERENCE - TYPE (java thì là object type) THÌ TỐN ĐẾN 2 VÙNG RAM. ---> 1 VÙNG CHO BIẾN OBJECT/BIẾN CON TRỎ - TRỎ VÙNG NEW (trỏ tới một địa chỉ) NẰM TRÊN HEAP CHỨA FULL INFO CỦA OBJECT
3. PHÂN VÙNG NHỚ TRONG RAM - Memory Segment 
	- ![](https://media.geeksforgeeks.org/wp-content/uploads/memoryLayoutC.jpg)
	   > Tham khảo [Memory Layout of C Programs](https://www.geeksforgeeks.org/memory-layout-of-c-program/)
	
	- ![](https://github.com/nguyenchiemminhvu/CPP-Tutorial/blob/master/8-con-tro/8-10-cac-phan-vung-tren-bo-nho-ao/0.png?raw=true)
	
	- <span style="color:#8d8d2a">Code segment hay text segment</span> - nơi lưu trữ các mã lệnh đã được biên dịch của các chương trình máy tính. Những mã lệnh trong phân vùng sẽ được chuyển đến CPU xử lý khi cần thiết. Code segment chịu sự chi phối của hệ điều hành
	- <span style="color:#8d8d2a">Data segment (initialize data segment)</span> - phân vùng mà hệ điều hành sử dụng để khởi tạo giá trị cho các biến kiểu static, biến toàn cục (global variable) của các chương trình.
	- <span style="color:#8d8d2a">BSS segment(uninitialized data segment)</span> - được dùng để lưu trữ các biến kiểu static, biến toàn cục nhưng chưa được khởi tạo giá trị.
	- <span style="color:#8d8d2a">Heap segment</span> - phần vùng được sử dụng để cấp phát bộ nhớ động thông qua kĩ thuật Dynamic memory allocation. Nơi lưu trữ các object sau khi toán tử new được thực hiện
		- Sau khi toán tử new thực thi thành công sễ trả về địa chỉ của vùng nhớ được cấp phát trên heap -> ta có thể sử dụng con trỏ có kiểu dữ liệu phù hợp để lưu trữ địa chỉ trả về này --> Con trỏ - công cụ duy nhất giúp ta truy cập chính xác địa chỉ của vùng nhớ lưu trữ thông tin đối tượng và truy xuất giá trị .
	- <span style="color:#8d8d2a">Stack Segment</span> - được dùng để cấp phát bộ nhớ cho tham số của các hàm(function parameters) và biến cục bộ trong các block. Hoạt động theo cấu trúc dữ liệu "Last in, First out"(LIFO), các lệnh rong khối sẽ được đưa vào trong stack và đưa vào vùng nhớ để được xử lý theo thứ tự  LIFO, xử lý xong hết các lệnh trong khối thì giải phóng bộ nhớ và đến lượt khối lệnh tiếp theo.
  
       >Nguồn tham khảo - [Nguồn](https://cpp.daynhauhoc.com/8/10-phan-loai-cac-vung-nho-stack-va-heap/)

	- > Biến static
     - ![](https://i.imgur.com/g3w7KgH.jpg)
     > id là trong class Student là một biến static - là một biến được nhìn thấy trong toàn app. Việc trích xuất tới nó phải thông qua tên class Student và gọi tới biến.` Student.id`. Nếu có sự thay đối đối với biến static id --> đồng nghĩa sự thay đổi này sẽ được nhìn thầy trong toàn bộ app.  
     > Cụ thể - ta khởi tạo 2 object của Student là s1 và s2. Trong đó s1 đã set giá trị cho id = 1, vậy Student.id đang = 1, nhưng s2 set id = 2 --> hệ lụy là id thay đổi = 2 -- vì là biến static - được sử dụng chung --> Student1.id bị thay đổi thành 2
## V. OOP - HƯỚNG ĐỐI TƯỢNG TRONG C# 
- Class trong C# - *<span style="color:#91819c">giống Java và các ngôn ngữ OOP khác</span>*
	- Tên class: Noun, Pascal Case
	- Bên trong chứa các đặc tính / trạng thái / mô tả / state
		* Gọi là : instance variable, backed-field, data-field
   > vd: private int _yob; private string _name;
 	  * <span style="color:#659532">Nếu đặc tính ko có từ "private" mặc định hệ thống hiểu là private cho đặc tính</span>
	 - Bên trong chứa các hành vi/behavior, method, function 
		* Có thể public, private. Mặc định không nói gì là private

#### 1. Kĩ thuật dùng PROPERTIES truy cập các đặc tính của class trong C# - [See]([](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties#properties-overview))
 > Trong class, các thuộc tính/fieds của một object thường được khai báo với quyền truy cập là private - chỉ cho phép truy cập trực tiếp trong class. 
 > --> Để cung cấp cơ chế đọc, ghi dữ liệu lên các thuộc tính private của một object 
 > --> C# cung cấp kĩ thuật PROPERTIES (hoạt động tương tự như getter và setter) hỗ trợ việc truy cập các thuộc tính của object một cách dễ dàng, an toàn và linh hoạt. 
 <span style="color:#d4a216">Có 3 kĩ thuật sử dụng đặc tính - properties của class</span>
 
##### Kĩ thuật 1: Kĩ thuật truyền thống Java
```CSharp
	private string _name; //backed-fied
	 
	public string GetName() 
	{
		return  _name;
	}
	    
	public string SetName(string name) 
	{
		this._name = name;
	}
```

- <span style="color:#0070c0">Hoặc dùng Expression Body</span>

```CSharp
	public string GetName() => _name;
	
	public void SetName(String name) 
	{
		_name = name;
	 }
```
			
##### Kĩ thuật 2: Kĩ thuật truyền giống Java nhưng Getter, Setter dùng kĩ thuật mới<span style="color:#d4a216"> (Properties with backing fields)</span> 

```CSharp
	//ví dụ 1
	private string _name; //backed field
	
	public string Name 
	{
		get {return _name; },
		set {_name = value; }
	}
```

```CSharp
	//ví dụ 2
	public class TimePeriod
	{
	    private double _seconds; //backed-fied
	    public double Hours
	    {
	        get { return _seconds / 3600; }
	        set
	        {
	            if (value < 0 || value > 24)
	                throw new ArgumentOutOfRangeException(nameof(value),
	                      "The valid range is between 0 and 24.");
	
	            _seconds = value * 3600;
	        }
	    }
	}
```
				
- <span style="color:#0070c0">PROPERTY VẪN DÙNG BACKED FIELD NHƯNG KẾT HỢP DÙNG EXPRESSION BODY</span>

```CSharp
//ví dụ 1
private string _name; //backed field

public string Name 
{
	get => _name;
	set => _name = value;
}
```
```CSharp
//ví dụ 2
public class Person
{
    private string _firstName; //backed-fied
    private string _lastName; //backed-fied

    public Person(string first, string last)
    {
        _firstName = first;
        _lastName = last;
    }

    public string Name => $"{_firstName} {_lastName}";
}
```
```CSharp
//ví dụ 3
public class SaleItem
{
    string _name; //backed-fied
    decimal _cost; //backed-fied

    public SaleItem(string name, decimal cost)
    {
        _name = name;
        _cost = cost;
    }

    public string Name
    {
        get => _name;
        set => _name = value;
    }

    public decimal Price
    {
        get => _cost;
        set => _cost = value;
    }
}
```


##### Kĩ thuật 3: Kĩ thuật ẩn đi backed
> Thực tế khi runtime, .NET tự add thêm ngầm getter, setter phía sau như kĩ thuật 2
```CSharp
public string Name {get; set;}
public decimal Price { get; set; }
```

<span style="color:#91819c">*=> KĨ THUẬT PROPERTY NÀY GIÚP NHÌN OBJECT TỰ NHIÊN HƠN SO VỚI JAVA*</span>

```Java
//ví dụ get set cho object tên hoang trong java

hoang.getName(); // lấy đc tên in đâu đó nếu muốn - gọi get() trực tiếp
hoang.setName("HoangNT"); 
```
```CSharp
//so sánh get set cho object tên hoang trong C#

hoang.Name; //Lấy tên được in đâu đó nếu muốn - gọi get() ngầm
hoang.Name = "HHOANG NNTT" // tự nhiên hơn, gán tên mới vào đặc tính Name
```
			  
				  												
#### 2. New mới một Object
- 1 Class có thể không làm constructor, khi đó, bạn vẫn new bình thường `new Tên-Class();` --> khi run-time, <span style="color:#d4a216">.NET sẽ TỰ TẠO CHO BẠN 1 **CONSTRUCTOR DEFAULT, RỖNG, KHÔNG THAM SỐ ĐẦU VÀO**</span>

>ví dụ
```CSharp
Student x = new Student();
Student y = new(); //bỏ luôn cả Student do đã biết trước đó y là Student rồi
```

 - <span style="color:#659532">Nếu định nghĩa sẵn các Property thì ta có quyền vừa new và gán PROTPERTY theo 2 kĩ thuật sau với sự hỗ trợ của <span style="color:#d4a216">CONSTRUCTOR DEFAULT</span> trong C#</span>
 
	- > KĨ THUẬT 1 - TRUYỀN THỐNG (Na ná JAVA)
		```CSharp
		Student x = new();
		x.Id = 1;
		x.Name = "HOANG";
		x.Yob = 2003;
		```

	- > KĨ THUẬT 2 - CÁCH ĐẸP C# HƠN
		```CSharp
		Student s1 = new Student { Id = 1, Name = "HOANGANH", Yob = 2003 };
		Student s2 = new() { Id = 1, Name = "HOANGANH", Yob = 2003 };
		Student s3 = new Student() { Id = 1, Name = "HOANGANH", Yob = 2003 };
		```
	-  <span style="color:#91819c">Đằng sau toán tử new là liệt kê các PROPERTY cần gắn value = value, nhét tất cả trong dấu ngoặc nhọn `{Value = Value}`</span>
	- <span style="color:#91819c"> 3 lệnh trên là new qua CONTRUCTOR DEFAULT(), ngoặc nhọn bên ngoài nghĩa là danh sách các setter PROPERTY được gọi để gắn value, vừa khai báo vừa gán value cho setter</span>

> [!Warning] Tạo riêng constructor bằng tay
> - Khi 1 class mà bạn chủ động tạo CONTRUCTOR DEFAULT thì dĩ nhiên C# không tự tạo thêm CONTRUCTOR DEFAULT ( vì 2 thằng trùng tên nhau là không được)
> - --
> - Khi bạn tạo thêm CONSTRUCTOR KHÁC RỖNG TỨC CONSTRUCTOR CÓ THAM SỐ --> .NET không tự tạo thêm CONSTRUCTOR RỖNG DEFAUT CHO BẠN. --> Lúc này không có constructor rỗng được tạo tự động bằng .NET nữa --> Nếu muốn có Constructor Rỗng -> phải tự tạo bằng tay - tức tạo thêm mới constructor rỗng riêng.

- <span style="color:#659532">Kĩ thuật New Constructor có tham số</span> trong C# 
	- > Ta cũng có nhiều cách new :
	
		```CSharp
			public class StudentName
			{
			    // Properties.
				public string? FirstName { get; set; }
				public string? LastName { get; set; }
			    public int ID { get; set; }
			    public override string ToString() => FirstName + "  " + ID;
			    
			    //Constructor Empty
			    public StudentName() { }
			    
				// The following constructor has parameters for two of the three
				// properties.
				public StudentName(string first, string last)
				{
					FirstName = first;
					LastName = last;
				}
			}
			    
			public class HowToObjectInitializers
			{
			    public static void Main()
			    {
				    //new object StudentName by using Constructor has 2 paramters name StudentName(string first, string last)
			        StudentName student1 = new StudentName("Craig", "Playstead");  // ==> new (dựa trên thứ tự tham số đầu vào ta khai báo value và hệ thống tự gán value theo thứ tự tham số đầu vào: value1, value2, value3,...); 
			        Student student11 = new StudentName(first: "Craig", last: "Playstead") //==> new (dựa trên tham số đầu vào ta gán tên tham số: value, tên tham số: value,...);
				        
			        //these following new objects with the help of EMTYP CONSTRUCTOR StudentName() { } same kĩ thuật 2 ở trên
			        StudentName student2 = new StudentName { FirstName = "Craig", LastName = "Playstead" };
			        StudentName student3 = new StudentName { ID = 183 };
			        StudentName student4 = new StudentName { FirstName = "Craig", LastName = "Playstead", ID = 116 };
			    }    
			}
		```



> [!Tip] new {} & ()
> - new object theo Constructor default thì <span style="color:#d4a216">khởi tạo các giá trị bằng cách gán các giá trị của các thuộc tính vào ngoặc nhọn</span> theo format {Tên property = value}
> - new object theo Constructor có tham số thì <span style="color:#d4a216">khởi tạo các giá trị trong ngoặc tròn ()</span>


## VI. TỔNG KẾT NHANH VỀ DATATYPE
1. Data Type là gì - Kiểu dữ liệu là gì ?
	- Là cách ta hoặc máy tính biểu diễn, thể hiển ra các thông tin quanh cuộc sống của và cách chúng được lưu trữ trong RAM, ví dụ các loại dữ liệu: số, chữ, ngày, tháng, đúng/sai, ...
2. C# cung cấp nhiểu loại kiểu dữ liệu khác nhau, tùy vào ý nghĩa, mục đích, đặc trưng của loại kiểu dữ liệu đó
	- 2.0 Xét theo tiêu chí biểu diễn thông tin ra bên ngoài cho ta nhìn thấy - ta có các kiểu dữ liệu sau:
		-> Số: số nguyên, số thực, số nhìn dạng nhị phân (binary), số thập phân (decimal), bát phân(octal), thập lục phân (hexa)
		-> Chữ: 1 kí tự nào đó
		-> Chuỗi: nhiều kí tự thành thành 1 từ, 1 câu
		-> Ngày tháng
		-> Đúng sai
		...
	- 2.1 Xét theo tiêu chí cách dữ liệu lưu trữ trong RAM, ta có 2 kiểu dữ liệu sau:
		-> VALUE TYPE: tham trị, tốn 1 vùng RAM để lưu giá trị 
		[BIẾN – VÙNG – RAM – LƯU – THẲNG – VALUE - LUÔN]
		> ví dụ: int (Int32), long (Int64), float(Single), double (Double)
		-> REFERENCE TYPE: tham chiếu, tốn 2 vùng ram để lưu giá trị 
		[BIẾN – “CON TRỎ”] --> TRỎ VÙNG NEW OBJECT TRONG HEAP - "tức là [OBJECT ĐƯỢC NEW]"
		> ví dụ: 
		> Object do hệ thống tạo sẵn: string(String), object(Object), Random, ArrayList, List<>,….
		> Custom object - object do người dùng đặt: ví dụ: Student, Lecture, DeadRacer, Rectangle, Shape, ... 
	- 2.2 Xét riêng cho kiểu REFERENCE - Kiểu tham chiếu trong C#
		- Có 2 loại kiểu tham chiếu:
		   + Có sẵn do .NET cung cấp sẵn: string, object, Random, List,…
		   + Do ta tự tạo ra để lưu trữ info nào đó : Student, Lecturer,…
		- Tạo dữ liệu kiểu tham chiếu ra dùng
		```CSharp
		public class XXX`
		{
			_backed fields;
			Properties;
			Methods(???); 
		//HÀM XỬ LÍ INFO GÌ ĐÓ, XỬ LÍ INFO CÓ SẴN BÊN TRONG CÁC _backedfields, BÊN TRONG Prop hoặc gọi hàm khác, hoặc nhận các tham số đưa vào
				}
		```
		```CSharp
		  public interface YYY
		   {
		   }
		```
		- 2 kiểu Class và Interface rất truyền thống giúp ta lưu trữ và xử lý các info --> Lưu trữ info qua biến/backed field và hàm/method
3. Kiểu dữ liệu loại DELEGATE - ỦY QUYỂN
	- C# đưa ra 1 cách khác biệt nữa để tạo ra 1 loại data type mới thay vì dùng để lưu trữ info và xử lý (class/interface), kiểu mới này nó đi sưu tập tên của các hàm mà ở đâu đó trong khắp cái app của mình. --> 1 kiểu dữ liệu mới, 1 từ khóa mới để tạo object chuyên đi gom tên của các hành động >>>> GỌI LÀ DELEGATE - ỦY QUYỀN
	> *Thay vì dùng Class, Interface để lưu cả info + hàm xử lí info. --> Bây giờ ta dùng delegate để tạo 1 không gian CHỈ ĐỂ **LƯU TRỮ TÊN CỦA CÁC HÀM***
	- 🔗 👉 Tìm hiểu về Delegate [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#VII. MỞ RỘNG SO VÓI OOP - DELEGATE & EVENT\|#VII. MỞ RỘNG SO VÓI OOP - DELEGATE & EVENT]]

