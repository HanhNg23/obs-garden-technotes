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

