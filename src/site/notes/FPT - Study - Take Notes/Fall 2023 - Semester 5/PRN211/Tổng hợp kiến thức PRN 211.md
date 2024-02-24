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

1. **<span style="color:#00b0f0">Kĩ thuật dùng PROPERTIES truy cập các đặc tính của class trong C#</span>** - [See]([](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties#properties-overview))
 > Trong class, các thuộc tính/fieds của một object thường được khai báo với quyền truy cập là private - chỉ cho phép truy cập trực tiếp trong class. 
 > --> Để cung cấp cơ chế đọc, ghi dữ liệu lên các thuộc tính private của một object 
 > --> C# cung cấp kĩ thuật PROPERTIES (hoạt động tương tự như getter và setter) hỗ trợ việc truy cập các thuộc tính của object một cách dễ dàng, an toàn và linh hoạt. 
 <span style="color:#d4a216">Có 3 kĩ thuật sử dụng đặc tính - properties của class</span>
 
**<span style="color:#00b0f0">1.1 Kĩ thuật 1: Kĩ thuật truyền thống Java</span>**
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
			
**<span style="color:#00b0f0">1.2 Kĩ thuật 2: Kĩ thuật truyền giống Java nhưng Getter, Setter dùng kĩ thuật mới (Properties with backing fields)</span>**

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


**<span style="color:#00b0f0">1.3 Kĩ thuật 3: Kĩ thuật ẩn đi backed</span>**
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

2. **<span style="color:#00b0f0">New mới một Object</span>**
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
1. **<span style="color:#00b0f0">Data Type là gì - Kiểu dữ liệu là gì ?</span>**
	- Là cách ta hoặc máy tính biểu diễn, thể hiển ra các thông tin quanh cuộc sống của và cách chúng được lưu trữ trong RAM, ví dụ các loại dữ liệu: số, chữ, ngày, tháng, đúng/sai, ...
2. **<span style="color:#00b0f0">C# cung cấp nhiểu loại kiểu dữ liệu khác nhau, tùy vào ý nghĩa, mục đích, đặc trưng của loại kiểu dữ liệu đó</span>**
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
3. **<span style="color:#00b0f0">Kiểu dữ liệu loại DELEGATE - ỦY QUYỂN</span>**
	- C# đưa ra 1 cách khác biệt nữa để tạo ra 1 loại data type mới thay vì dùng để lưu trữ info và xử lý (class/interface), kiểu mới này nó đi sưu tập tên của các hàm mà ở đâu đó trong khắp cái app của mình. --> 1 kiểu dữ liệu mới, 1 từ khóa mới để tạo object chuyên đi gom tên của các hành động >>>> GỌI LÀ DELEGATE - ỦY QUYỀN
	> *Thay vì dùng Class, Interface để lưu cả info + hàm xử lí info. --> Bây giờ ta dùng delegate để tạo 1 không gian CHỈ ĐỂ **LƯU TRỮ TÊN CỦA CÁC HÀM***
	- 🔗 👉 Tìm hiểu về Delegate [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#VII. MỞ RỘNG SO VÓI OOP - DELEGATE & EVENT\|#VII. MỞ RỘNG SO VÓI OOP - DELEGATE & EVENT]]


## VII. MỞ RỘNG SO VỚI OOP - DELEGATE & EVENT
#### 1. KHÁI NIỆM DELEGATE

<span style="color:#8d8d2a">**Giới thiệu**</span> [See](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/#delegates-overview)

- Khái niệm Datatype -> khái quát - gom và đặt tên
- Data type - đại diện một tập hợp chung. Ví dụ tập hợp kiểu int - data type tên int - đây là tên đại diện cho một tập hợp gồm các số nguyên. 
	- -> Định nghĩa tập hợp thuộc kiểu int là tập hợp các giá trị nguyên thì tập hợp biểu diễn như sau: int là 4 byte có dấu -> tập hợp giá trị của tập hợp tên int {-2,147,483,648, 2,147,483,647}
	- -> Khi muốn gọi đến một phần tử cụ thể trong tập hợp, tuy nhiên ta chưa biết cụ thể phần tử cần gọi là phần tử nào --> ta sẽ tạo ra 1 BIẾN tên là x - với x thuộc tập hợp int - viết code: `int x;` --> x sẽ là một phần tử bất kì nào đó trong tập hợp int --> Biến x này sẽ được dùng để trỏ đến một phần tử thuộc tập hợp int trên. <span style="color:#555555">ví dụ x = 4; --> x trỏ đến phần tử 4 của tập hợp int</span>
	- Tương tự - Delegate sẽ hoạt động theo cách trên với mục đích sử dụng là tạo ra được một tập hợp hàm mới - tập hợp các phần tử là các hàm trong một class có cùng kiểu dữ liệu trả về, cùng kiểu tham số truyền vào.
==> *Vậy bản thân Delegate là một loại đại diện cho tập hợp các biến tham chiếu thuộc kiểu Delegate - từng biến tham chiếu sẽ tham chiếu đến tập hợp hàm của riêng nó được tạo bởi Delegate*

<span style="font-weight:bold; color:#8d8d2a">**Hình dung mô tả ?**</span>

Hàm là một object cần khái quát

| Hàm kiểu void                                       | Hàm kiểu retrun                                    | Hàm kiểu trả về boolean                             |                   |
| --------------------------------------------------- | -------------------------------------------------- | --------------------------------------------------- | ----------------- |
| void FV1(){}                                        | int FR1(){}                                        | bool FB(){}                                         | ...nhiều hàm khác |
| void FV2(){}                                        | int FR2(){}                                        | bool FB2(){}                                        |                   |
| style FUNCTION trả về void                          | style FUNCTION trả về int                          | style FUNCTION trả về bool                          |                   |
| <span style="color:#8d8d2a">style void FV();</span> | <span style="color:#8d8d2a">style int FR();</span> | <span style="color:#8d8d2a">style bool FB();</span> |                   |
| 👆 tên gọi đại diện nhóm                            | 👆 tên gọi đại diện nhóm                           | 👆 tên gọi đại diện nhóm                           |                   |
| hàm cùng style void - void                          | hàm cùng style int - void                          | hàm cùng style bool-void                            |                   |
|                                                     |                                                    |                                                     |                   |

<span style="color:#d4a216">===> *ĐẠI DIỆN CHO MỘT ĐỐNG CÁC HÀM CÓ CÙNG STYLE --> gọi là DELEGATE*</span>
- <span style="color:#555555">ví dụ ở đây có 3 đại diện: đại diện hàm void - void FV();, đại diện hàm int - int FR(); đại diện hàm bool - bool FB();</span>
	- <span style="color:#555555"> void - void -> chỉ loại hàm có kiểu trả về là kiểu void, ko nhận tham số gì thì gọi là void</span>
	- <span style="color:#555555">int - void -> chỉ loại hàm có kiểu trả về là int, ko nhận tham số gì thì gọi là void</span>
	- <span style="color:#555555">bool - void -> tương tự</span>

<span style="color:#8d8d2a">**Hình minh họa hoạt động của delegate**</span> 
	
![](https://i.imgur.com/AQ7KdlL.png)

<span style="color:#9a7db0">==> Qua hình trên ta đã khái quát ra được 2 DATA TYPE = 2 loại delegate là FV và FR</span>

> [!NOTE] 
> Theo định nghĩa của C#, Delegate là một loại chuyên đóng gói một phương thức nào đó ~~ tương tự như hàm con trỏ trong C và C++. Nó khác hàm con trỏ C ở chỗ là các delegates này là object-oriented, type safe và secure. Các delegates này được tạo ra bởi class Delegate trong .NET. Tên của loại delegate đó sẽ phụ thuộc vào tên mà ta định nghĩa cho delegate trên.

 <span style="color:#8d8d2a">**Gọi hàm cụ thể**</span>
> [!QUESTION] Khi tập hợp các hàm vào trong nhóm, làm sao để lấy 1 thằng trong nhóm ra nói chuyện ?
> Khi ta khái quát để xây dựng DATA TYPE --> Sau đó ta TÁCH BẠCH để DATATYPE VARIABLE = VALUE với VALUE là từ 1 trong những thằng thuộc tập hợp DATATYPE đó, VARIABLE sẽ là một tên gọi đại diện cho tập hợp DATA TYPE đó ?

<span style="color:#00b0f0">**Trả lời**</span>

**ỨNG DỤNG khái niệm TÁCH BẠCH để gọi hàm cụ thể** 

- TÁCH BẠCH -- đề cập đến cụ thể  == đề cập đến một hàm cụ thể nào đó để sử dụng
- <span style="color:#9a7db0">**Ví dụ lấy các đối tượng từ "hình minh họa hoạt động của delegate" ở trên**</span>
	- Nói FV x; --> sắp nói đến một hàm void - void --> x đại diện cho một phần tử bất kì thuộc tập hợp FV --> x thuộc FV
	- x = FV1; --> gán tên hàm là FV1 cho x (<span style="color:#555555">FV1 này là một phần tử nằm trong tập hợp FV</span>) --> Vì FV1 là hàm kiểu void - void nên x sẽ trỏ đến hàm FV1 thông qua tên gọi hàm là "FV1" (<span style="color:#00b0f0">*Chú ý: gọi hàm nhưng không thêm ngoặc '()'*</span>) 
	- Tương tự: Nói FR x = FR2 --> x là một hàm FR2 == x sẽ trỏ đến hàm FR2 thuộc tập hợp hàm FR có style int - void

> [!SUCCESS] Gọi hàm trong Delegate
> - Bình thường gọi hàm ko dùng Delegate là gọi hàm bằng **tên hàm kèm DẤU "()" và trong truyền tham số**; (<span style="color:#6a5858">vd: obj.hàm(arguments))</span> 
> - <span style="color:#8d8d2a">Khi dùng DELEGATE --> ta không gọi hàm trực tiếp - ko gọi hàm có dấu "()" --> MÀ **GỌI QUA TÊN HÀM CỤ THỂ**</span>
> ví dụ: FR y = FR2 --> gọi hàm cụ thể qua tên hàm là FR2 được lấy ra trong tập hợp FR 
> - <span style="color:#8d8d2a">Ngoài ra, DELEGATE còn có tính năng mở rộng của việc gọi hàm -</span> [Multicast Delegates](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/how-to-combine-delegates-multicast-delegates)
> 	- Nếu ta có int x = 4; ta có thể chơi x += 5 == x = (current) x + 5 
> 	--> DELEGAT cũng có thể làm điều đó bằng cách FV y = FV1; y += FV1; y =+ FV2 --> bảo y trỏ đến hàm FV1 - thực thi xong - trỏ hàm FV1 tiếp theo thực thi tiếp - thực thi xong - trỏ hàm FV2 thực thi.


> [!NOTE]- Code ví dụ minh họa sử dụng Delegate  [See](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/using-delegates)
>    ```CSharp
>    //Khai báo một biến tham chiếu thuộc kiểu Delegate = khai báo một thằng delegate / một biến thuộc Delegate và đặt tên là Callback - biến/delegate này dùng tham chiếu đến các hàm có kiểu dữ liệu trả về là void và tham số truyền vào là string. --> quá trình này đồng nghĩa là quá trình tạo ra một Data Type mới - một tập hợp mới (tập hợp các hàm kiểu void - string) có tên Callback 
> 	public delegate void Callback(string message);
> 	
> 	// Create a method for a delegate.
> 	public static void DelegateMethod(string message)
> 	{
> 	    Console.WriteLine(message);
> 	}
> 	// Instantiate the delegate - khởi tạo ra một biến - một delegate có tên là Callback. Biến handler dưới đây tham chiếu tới hàm DelegateMethod
> 	Callback handler = DelegateMethod;
> 	
> 	// Call the delegate and pass string parameter
> 	handler("Hello World");
> 	
> 	//Use Delegate to pass method to another method as an argument
> 	public static void MethodWithCallback(int param1, int param2, Callback callback)
> 	{
> 	    callback("The number is: " + (param1 + param2).ToString());
> 	}
> 	//pass the `handler` type `DelegateMethod` as a parameter of `MethodWithCallback` method
> 	MethodWithCallback(1, 2, handler);


#### 2. KHAI BÁO, KHỞI TẠO VÀ SỬ DỤNG DELEGATE [See](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/how-to-declare-instantiate-and-use-a-delegate)
 
1. <span style="color:#8d8d2a">**Sử dụng Named Method**</span>
	- Named method là ta đang định nghĩa sẵn một hàm. Khi khởi tạo một loại delegate nào đó thì ta có thể sử dụng hàm được định nghĩa sẵn và truyền hàm đó như một parameter.
    - >ví dụ
		```Csharp
			// Declare a delegate.
			delegate void WorkCallback(int x);
					
			// Define a named method.
			void DoWork(int k) { /* ... */ }
					
			// Instantiate the delegate using the method as a parameter.
			WorkCallback d = obj.DoWork;
		```


2. <span style="color:#8d8d2a">**Sử dụng Anonymous Method**</span>
	- Vì Delegate sinh ra để trỏ, lưu trữ địa chỉ của hàm nào đó = Tên-hàm-nào-đó() và sau đó delegate sẽ gọi hàm này thay cho gọi hàm trực tiếp
	- Nhưng có tình huống ta làm biếng tạo sẵn hàm - named method - một hàm có tên gọi đàng hoàng --> LÀM BIẾNG ĐẶT TÊN HÀM, NHƯNG VẪN MUỐN NHỜ DELEGATE GỌI GIÙM đến một hàm chỉ có mã lệnh chứ không khai báo hàm cụ thể (có tên) --> Anonymous method RA ĐỜI
	- Anonymous method - C# cho phép bạn khởi tạo một delegate bằng cách định nghĩa trực tiếp một code block mà ta muốn delegate xử lý. <span style="color:#555555">*(Codeblock này có thể viết theo kiểu lambda expression || anonymous method và codeblock phải đúng style của delegate trên.)*</span>
	- >ví dụ - declare an anonymous method:
		```CSharp
		FDelegate fNaoDo = delegate(int x)
		{
			//nội dung hàm nằm ở đây || xử lí hàm nằm ở đây
		}
		// Instantiate NotifyCallback by using an anonymous method.
		NotifyCallback del3 = delegate(string name)
		    { Console.WriteLine($"Notification received for: {name}"); };
		```

3. <span style="color:#8d8d2a">**Sử dụng Lambda Expression**</span> - <span style="color:#91819c">Nhìn hàm mà giống biểu thức tính toán</span>
	- Là trường hợp đặc biệt của ANONYMOUS FUNCTION - Tức là nó cũng là hàn ẩn danh - anonymous method - nhưng được viết ở mức độ rút gọn nhất có thể
	---
	- <span style="color:#91819c">B1. CĂN ĐẦU VÀO CỦA BIỂU THỨC Y CHANG ĐẦU VÀO CỦA HÀM ĐANG ĐƯỢC TRỎ</span>
	 Ví dụ nếu hàm đang trỏ có kiểu Void-Int - trả về Void và nhận tham số kiểu Int --> thì biểu Lambda khởi đầu = Tham số hàm. Nếu hàm ko có tham số đầu vào thì dùng cặp ngoặc tròn `()`
		```CSharp
		FDelegate fNaoDo = BIEU THUC LABDA
		FDelegate fNaoDo = (int x) //cho hàm có nhận tham số
		FDelegate! fNaoDo = () //cho hàm không yêu cầu tham số
		```
	
	- <span style="color:#91819c">B2. SAU THAM SỐ HÀM LÀ MŨI TÊN => TRỎ ĐẾN THÂN HÀM</span>
		```CSharp
		FDelegate fNaoDo = (int x) =>
		FDelegate! fNaoDo = () =>
		FDelegate fNaoDo = (int x, int y) =>	
		```
	
	- <span style="color:#91819c">B3. NẾU THÂN HÀM CHỈ CÓ 1 LỆNH DUY NHẤT , THÌ KO CẦN NGOẶC NHỌN {}
		CÒN THÂN HÀM CÓ NHIỀU HƠN 1 LỆNH -> THÌ BẮT BUỘC PHẢI {BODY HÀM}</span>
		```CSharp
		FDelegate fNaoDo = (int x) => 1 LỆNH NÒA ĐÓ
		FDelegate fNaoDo = (int x) => 
		{
			NHIỀU LỆNH VIẾT NHƯ HÀM BÌNH THƯỜNG;
		}
		```
	
> [!TIP]-  <span style="color:#8d8d2a">**Quy tắc rút gọn tối đa**</span>
> - Nếu thân hàm chỉ có 1 lệnh --> Không cần dùng {}, nếu lệnh có return, bỏ return luôn
> () => 1 lệnh;
> (int x) => 1 lệnh;
> ---
> - Nếu thân hàm có từ 2 lệnh trở lên, bắt buộc phải {Body hàm} và kèm return như hàm bình thường, ko ưu tiên gì cả
> () => {
> 		code các lệnh;
> 		return ???;
>       }
> ---
> - Nếu đầu vào không có gì cả, thì sẽ là () =>
> - Nếu đầu vào có 1 tham số, bỏ luôn dấu ngoặc truyền tham số `()` và bỏ luôn cả kiểu dữ liệu. Rút gọn và ghi là : s => lệnh, x => lệnh, a => lệnh,...(với s, x , a đại diện cho tham số đầu vào)
> - Nếu đầu vào từ 2 3 tham số, bỏ luôn kiểu dữ liệu, nhưng phải ghi tên tham số trong ngoặc (các tham số đầu vào)
>   (a, b) => lệnh
>   (a, b, c) => lệnh

🔗[Lambda expressions Operator in C#](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions)

#### 4. ỨNG DỤNG CỦA DELEGATE - DELEGATE DÙNG ĐỂ LÀM GÌ ??
1. <span style="color:#8d8d2a">**Dùng để mở rộng khả năng của 1 object bất kì**</span>
	- Tức là 1 Object được thiết kế trước đó, có thể làm được thêm nhiều công việc mà lúc thiết kế ra nó, người ta chưa nghĩ ra, hoặc dự kiến object sẽ làm được 1 điều gì đó nhưng chưa biết cụ thể là gì -> đến 1 lúc nào đó sẽ làm được (LOOSE COUPING - gắn kết lỏng lẻo)
2. <span style="color:#8d8d2a">**Dùng để xử lý các sự kiện/event**</span> [See](https://howkteam.vn/course/khoa-hoc-lap-trinh-c-nang-cao/event-voi-delegate-trong-c-4041) 
	- Ý tưởng của event -> mỗi khi đụng tới object tức điều gì đó xảy ra trên Object (đặc biệt là Object liên quan đến Windowform/App Desktop 
	  --> sẽ tự động gọi tới tất cả các sự kiện/hành động tương ứng với các sự thay đổi của object này 
	  [Ví dụ sự kiện Button.DoubleClick Event của class Button](https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms.button.doubleclick?view=windowsdesktop-8.0)
	- Các Object trên Windowform thì nó là 1 Object gần hoàn chỉnh
	- Nó có các đặc tính, hành động như mọi class
	  > ví dụ instance của Class Button [See](https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms.button?view=windowsdesktop-8.0)
		```CSharp
		private Button btnSave;
		btnSave = new Button();
		btnSave.Location = new Point(719, 482);
		btnSave.Name = "btnSave";
		btnSave.Size = new Size(75, 23);
		btnSave.TabIndex = 21;
		btnSave.Text = "Save";
		btnSave.UseVisualStyleBackColor = true;
		btnSave.Click += btnSave_Click;		
		```
	- Gọi là gần hoàn chỉnh vì nó hiện thị trên màn hình ngon lành, nhưng nó còn vô dụng - click vào nó không biết làm gì vì phần này tùy thuộc vào ai đó - ai đó là dev xài nút này khi viết code
	- --> Để nút bấm làm được điều gì thì cần dev viết nốt đoạn code làm gì khi bị click
		```CSharp
			class Button
			{
				- name: ____
				- text: ____
				- color: _____
				Click(tên hàm here, delegate nhận vào, xử-lý-gì-đó)
				{
				   làm gì đâu biết, chừa sẵn chỗ để nhét hàm khi có ai xài nút này, dev nào xài thì đưa code vào
				   -> Đưa code vào đưa hàm vào
				   Gọi hàm của dev bên ngoài đưa vào() //biến delegate 'nhận vào' trỏ tới 'tên hàm' để yêu cầu xử lý
				}
			}
		
			HàmXửLýClick() 
			{
				code của dev xài nút bấm làm cái gì đó
			}
			//hậu trường xử lý kéo thả nút bấm, code, property
			Click += HàmXửLýClick;   //gán con trỏ hàm cho sự kiện Click chừa chỗ
			//Tui click button mở rộng khả năng của tôi, chấp nhận xài hàm của anh em bên ngoài, tui button đưa hàm cho button tui đi, tui thay bạn gọi hàm thực thi cho
			//Window kiểm soát click, gọi nút bấm click khi user nhấn nút, nút được windows đẩy chạy click
		```

## VIII. WINDOWS FORMS VÀ STYLE VIẾT CODE
- Cách viết code hiện nay gọi là style ALL-IN-ONE (Nhét tất vào một chỗ)
	- 1 project của 1 solution chứa toàn bộ form - UI - trong form chứa code xử lý sự kiện + chứa luôn data đứng sau form 
	- 1 form của 1 project chứa vừa design & code, event & code data luôn
> <span style="color:#6a5858">Nếu ta cần data không phải từ Ram mà là từ database SQLServer, code nên sửa ntn, All-In-One còn đủ tốt không</span>
> <span style="color:#6a5858">--> Không tốt vì code event trộn với code xử lý data (code nghiệp vụ khách hàng: không mượn quá 5 cuốn, tính giảm giá)</span>
> <span style="color:#6a5858">--> Không đủ tốt vì fix với SQLServer</span>
> <span style="color:#6a5858">--> Nếu Data không từ SQLServer mà từ MySQL, ....</span> 
> <span style="color:#6a5858">--> Code phải copy paste sang dự án khác, sửa - dân dev gặp nhiều ác mộng trong đó ác mộng maintain 2 app đồng thời SQLServer, MySQL. UI thì giống, DB khác, câu lệnh Select khác --> Dẫn tới tái sử dụng kém</span> 

---
**TỐI ƯU HÓA TỔ CHỨC CODE**
- --> Tách UI (Form và Event) ra riêng
- --> Xử lý data ra 1 chỗ riêng 
---
#### Tổ chức code theo mô hình kiến trúc 3 lớp 
Mô hình 3 lớp - layer gồm có 3 thành phần chính:

<p style="text-align:center;">
<img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/20200103194305/NET-3-Tier-Architecture.png" width="40%" height="auto"/>
</p>

- Presentation Layer (GUI): lớp có nhiệm vụ chính giao tiếp với người dùng, tương tác với người dùng. Nó gồm các thành phần giao diện (win form, web form,...) và thực hiện các công việc như nhập liệu, hiển thị dữ liệu, kiểm tra tính đúng đắn về format của dữ liệu trước khi gọi lớp Business Logic Layer (BLL) xử lý.
- Business Logic Layer (BLL): layer phân ra thành 2 nhiệm vụ:
	- Nơi đáp ứng các yêu cầu đối với thao tác dữ liệu gửi từ GUI layer, xử lý chính nguồn dữ liệu từ Presentation Layer trước khi truyền xuống Data Access Layer và lưu xuống hệ quản trị CSDL.
	- Nơi kiểm tra các ràng buộc, tính toàn vẹn và hợp lệ dữ liệu, thực hiện tính toán - xử lý các yêu cầu nghiệp vụ trước khi trả kết quả về Presentation Layer
   <span style="color:#00b0f0">**Trong C# web app ta xây dựng lớp này qua project Service**</span>
- Data Access Layer (DAL): Lớp có chức năng giao tiếp với hệ quản trị CSDL như thực hiện các công việc liên quan đến lưu trữ và truy vấn CRUD dữ liệu
 <span style="color:#9a7db0">**Trong C# web app ta xây dựng lớp này qua project Repository**</span>
    
<span style="color:#00b0f0">🔗 Nguồn:[Mô hình 3 lớp - top dev](https://topdev.vn/blog/mo-hinh-3-lop-la-gi/)</span>

#### Tư duy thiết kế code - cấu trúc code của bài FAP.V2

| UI                              | Chứa data phục vụ Form           | Định dạng dữ liệu phục vụ                                    |
| ------------------------------- | -------------------------------- | ------------------------------------------------------------ |
| Class Form ListStudents.cs - UI | Class StudentRepositorySqlServer | Class Student Id, Name, Yob  {get; set;} cho từng thuộc tính |
| StudentRepoSqlServer `_repo`    | List`<Student>` `_ds`;           |                                                              |
| dgvStudentList                  | GetAll() -> List\<Student\>        |                                                              |
| btnAdd()                        | Add(Student x)                   |                                                              |
| btnUpdate()                     | Update(Student x)                |                                                              |
| btnDelete()                     | Delete(id)                       |                                                              |
| btnSearch()                     | Search(id)                       |                                                              |
|                                 |                                  |                                                              |

> [!QUESTION]+ Nhận xét 
> 🌸 Muốn xài SQLServer thì dùng class StudentRepositorySqlServer
> 🌸 Muốn xài MySQLServer thì dùng class StudentRepositoryMySqlServer
> 
> ↪ 🤔 Nhưng vấn đề hình dung cả 2 class trên đếu có chức năng chính là thao tác dữ liệu với database khác nhau ở hệ cơ sở dữ liệu, nhưng chắc chắc sẽ có các phương thức chung như là kết nối database, truy vấn CRUD lên database,...==> 2 class xài chung một outline
> 
> ⚒️ Tới đây ta nâng cấp code, cho 2 class trên tuân thủ một outline - ta sẽ tạo dùng INTERFACE với mục đích cung cấp một lớp làm mẫu chứa outline, các lớp học theo mẫu này chỉ có nhiệm vụ triển khai phát triển lên từ lớp mẫu.
> 
> ⚙️ ví dụ: Lớp Cha là interface có 2 phương thức cần lớp con triển khai là ChàoChaMẹTrướcKhiĐiHọc(), ChàoChaMẹKhiĐiHọcVề() --> Lớp Con là Class - Con 1 , Con 2 đều phải triển khai 2 phương thức trên và nếu muốn nó có thể triển khai thêm các phương thức khác của riêng nó



| UI                                                                             | Chứa data phục vụ Form                                                                           | Định dạng dữ liệu phục vụ                                    |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------ |
| Class Form ListStudents.cs - UI                                                | Class StudentRepositorySqlServer OR StudentRepositoryMySqlServer - implement -> IStudentRepository | Class Student Id, Name, Yob  {get; set;} cho từng thuộc tính |
| IStudentRepository _repo                                                       | List`<Student>` `_ds`;                                                                           |                                                              |
| (👆 _repo. sẽ ăn theo các hàm bên lớp con mà đã triển khai từ Interface Cha 👉 |                                                                                                  |                                                              |
| dgvStudentList                                                                 | GetAll() -> List\<Student\>                                                                        |                                                              |
| btnAdd()                                                                       | Add(Student x)                                                                                   |                                                              |
| btnUpdate()                                                                    | Update(Student x)                                                                                |                                                              |
| btnDelete()                                                                    | Delete(id)                                                                                       |                                                              |
| btnSearch()                                                                    | Search(id)                                                                                       |                                                              |

> [!SUMMARY]+ Tổng quát
> <span style="color:#9a7db0">Class StudentRepositoryMySQL : IStudentRepository</span>
> <span style="color:#9a7db0">Class StudentRepositorySqlServer : IStudentRepository</span>
> 	
> <span style="color:#9a7db0">Class Form ListStudent.cs - UI</span>
> 	<span style="color:#9a7db0">IStudentRepository _repo = StudentRepositoryMySQL()</span>
> 	<span style="color:#9a7db0">IStuentRepository _repo = StudentRepositorySqlServer();</span>
> 	
> 	===> nếu muốn dùng GUI Form ListStudent thì các class triển khai repository bắt buộc phải implement **Interface IStudentRepository** vì GUI chỉ chơi với thằng là instance hay là con của IStudetnRepository
> 	==> Đây gọi là các **DEPENDENCY INJECTION**
> 	==> Trong thực tế, người ta thường khai báo Interface để đảm bảo các thằng con implement thằng cha phải đồng nhất các chức năng cơ bản cần bắt buộc khai báo.