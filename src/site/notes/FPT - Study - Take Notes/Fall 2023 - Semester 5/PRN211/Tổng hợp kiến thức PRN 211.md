---
{"dg-publish":true,"dg-permalink":"FPT-Prn211-Review","permalink":"/FPT-Prn211-Review/"}
---


# 🗒️ DOT NET PRN211
---
> [!SUMMARY]+ Nội dung
> [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#TIỀN BÀI HỌC - REVIEW KIẾN THỨC VỀ JAVA\|#TIỀN BÀI HỌC - REVIEW KIẾN THỨC VỀ JAVA]]
> [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#I. .NET, .NET FRAMEWORK, .NET CORE\|#I. .NET, .NET FRAMEWORK, .NET CORE]]
> [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#II. PHÂN BIỆT PROJECT, SOLUTION\|#II. PHÂN BIỆT PROJECT, SOLUTION]]
> [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#III. CODING CONVENTION - QUY ƯỚC ĐẶT TÊN TRONG DỰ ÁN\|#III. CODING CONVENTION - QUY ƯỚC ĐẶT TÊN TRONG DỰ ÁN]]
> [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#III. CODING CONVENTION - QUY ƯỚC ĐẶT TÊN TRONG DỰ ÁN\|#III. CODING CONVENTION - QUY ƯỚC ĐẶT TÊN TRONG DỰ ÁN]]
> [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#V. OOP - HƯỚNG ĐỐI TƯỢNG TRONG C\|#V. OOP - HƯỚNG ĐỐI TƯỢNG TRONG C]]
> [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#VI. TỔNG KẾT NHANH VỀ DATATYPE\|#VI. TỔNG KẾT NHANH VỀ DATATYPE]]
> [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#VII. MỞ RỘNG SO VỚI OOP - DELEGATE & EVENT\|#VII. MỞ RỘNG SO VỚI OOP - DELEGATE & EVENT]]
> [[FPT - Study - Take Notes/Fall 2023 - Semester 5/PRN211/Tổng hợp kiến thức PRN 211#VIII. WINDOWS FORMS VÀ STYLE VIẾT CODE\|#VIII. WINDOWS FORMS VÀ STYLE VIẾT CODE]]

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
