# Automated Analysis Of Malicious Microsoft Office Documents

[Reference Paper Link](https://www.sciencedirect.com/science/article/pii/S0167404821004053?)


    Keywords:
    Malware
    Office documents
    Macro malware
    Powershell
    LOLBAS

- Abstract:

`Cho đến nay, Microsoft Office có thể là bộ ứng dụng được sử dụng rộng rãi nhất để xử lý tài liệu, bảng tính và bản trình bày. Do tính phổ biến của nó, nó liên tục được sử dụng để thực hiện các chiến dịch độc hại.` Các tác nhân đe dọa, khai thác các tính năng động của nền tảng, sử dụng nó để khởi động các cuộc tấn công và thâm nhập hàng triệu máy chủ trong chiến dịch của họ.
<br><br>
Công việc này khám phá bối cảnh hiện đại của các tài liệu Microsoft Office độc ​​hại, chỉ ra các phương tiện mà các tác giả phần mềm độc hại sử dụng. Chúng tôi tận dụng sự phân loại của các công cụ được sử dụng để xử lý các tài liệu Microsoft Office và khám phá hoạt động của các tác nhân độc hại. Hơn nữa, chúng tôi đã tạo và chia sẻ công khai một `tập dữ liệu được chế tạo đặc biệt, dựa trên việc kết hợp các tài liệu lành tính và độc hại có chứa nhiều tính năng động như macro VBA và DDE.` Điều sau là rất quan trọng cho một phân tích công bằng và thực tế, một vấn đề mở trong tình trạng nghệ thuật hiện nay. Điều này cho phép chúng tôi đưa ra kết luận an toàn về các tính năng và hành vi độc hại. Chính xác hơn, chúng tôi trích xuất các tính năng cần thiết bằng quy trình phân tích tự động để phân loại hiệu quả và chính xác tài liệu là lành tính hay độc hại bằng cách sử dụng máy học với điểm F1 trên 0,98, vượt trội so với các thuật toán phát hiện hiện đại nhất.

## 1. Introduction:

`Microsoft Office cho đến nay vẫn là bộ ứng dụng văn phòng được sử dụng rộng rãi nhất.` Các tài liệu được tạo ra từ bộ phần mềm không có nghĩa là tĩnh và chứa nhiều yếu tố động để làm cho chúng trở nên dễ chịu hơn về mặt thẩm mỹ, cung cấp các chức năng và tương tác nâng cao. Bản chất động của các phần tử được tăng cường thêm khi sử dụng ngôn ngữ lập trình `VBA (Visual Basic for Applications).` Tuy nhiên, nhiều lợi ích mà sự hỗ trợ từ một ngôn ngữ như vậy có thể mang lại nhưng phải trả một cái giá đắt. Lý do là kẻ thù có thể bọc giáp các tài liệu để tiến hành một cuộc tấn công. Trên thực tế, các tài liệu văn phòng độc hại được các thực thể độc hại sử dụng rộng rãi để phát tán phần mềm độc hại trên toàn thế giới, thường là thông qua email. Do được chuyển lại bởi nhiều nguồn khác nhau (Avira, 2020; ESET, 2020), tài liệu MS Office là định dạng tệp được phần mềm độc hại cho Windows sử dụng rộng rãi thứ hai và liên tục được sử dụng trong các chiến dịch malspam (Crowdstrike, 2020; Patsakis và Chrysanthou, 2020) .
<br><br>
`Lý do chính mà các tệp này được sử dụng thường xuyên trong các chiến dịch như vậy là chúng được người dùng trao đổi liên tục. Do đó, người dùng có nhiều khả năng sẽ tải xuống và mở tệp MS Office mà họ nhận được, ngay cả từ một người gửi không xác định,` ví dụ: tệp thực thi hoặc định dạng tệp “lạ”. Về vấn đề này, một tệp MS Office được kẻ thù sử dụng để đặt chân vào máy của nạn nhân và sau đó tiến hành lây nhiễm thực sự cho máy chủ. `Trong khi AV có nhiều biện pháp kiểm tra cụ thể để ngăn chặn sự lây nhiễm từ các tệp như vậy, ngoài một số biện pháp kiểm soát được chế tạo đặc biệt từ MS cho phép người dùng cấp quyền thực thi một cách có chọn lọc, người dùng vẫn trở thành nạn nhân của các cuộc tấn công này.` Đáng chú ý, một trong những phần mềm độc hại khét tiếng nhất trên toàn thế giới gần đây đã bị gỡ xuống Malwarebytes, Emotet, đang sử dụng các tài liệu MS Office bọc thép để lây nhiễm.

### 1.1. Motivation and contribution:

Mục tiêu của công việc này là thực hiện phân tích kỹ lưỡng các tài liệu MS Office độc ​​hại cho đến nay và thiết lập hiểu biết cơ bản về hoạt động mô-đun của chúng thông qua một cách tiếp cận tự động. Để đạt được mục tiêu này, chúng tôi đã thiết lập một `quy trình tự động bao gồm một tập hợp các bước phân tích tài liệu cả tĩnh và động, đồng thời trích xuất một tập hợp các tính năng có thể được sử dụng để tạo điều kiện phân loại tài liệu từ lành tính đến độc hại.`
<br><br>
Sự đóng góp của công việc này là rất nhiều. Phần lớn công việc trong các tài liệu MS Office độc ​​hại đang tập trung vào các tính năng có thể được sử dụng để phát hiện các tài liệu độc hại, nhưng không tập trung vào lý do thực tế tại sao chúng tồn tại. Trong bối cảnh này, chúng tôi thực sự tiết lộ những gì một tài liệu độc hại làm. Trái ngược với việc chỉ đơn giản nói rằng, ví dụ: chuỗi hex được sử dụng, thông qua thử nghiệm mở rộng, chúng tôi cho thấy rằng mặc dù có khả năng tiềm ẩn, nhưng các tài liệu MS Office độc ​​hại này thực sự là các ống nhỏ giọt; họ tải xuống và thực thi một tải trọng độc hại. Trong bối cảnh này, bài báo của chúng tôi là bài báo đầu tiên định lượng vấn đề thể hiện `những phương tiện nào để thực hiện tải trọng bị giảm.` Theo hiểu biết tốt nhất của chúng tôi, đây là bài báo đầu tiên trong tài liệu học thuật `sử dụng giải mã để chỉ ra điều này trong khi hầu hết các tác giả chỉ đơn giản coi việc làm nhiễu loạn là một dấu hiệu của sự độc hại. Điều thứ hai có nghĩa là họ phát hiện ra triệu chứng nhưng không phát hiện ra nguyên nhân của nó. Ngoài ra, ngoài việc chỉ báo cáo LOLBAS / LOLBIN đã sử dụng, chúng tôi cho thấy sự đa dạng của các lựa chọn này cho thấy rằng một số giải pháp EDR và ​​EPP, mặc dù biết vectơ tấn công này không chặn chúng một cách hiệu quả.` Điều sau biện minh cho tác động cao của các chiến dịch phần mềm độc hại sử dụng cách tiếp cận này.
<br><br>
Một đóng góp rất quan trọng cũng là tập dữ liệu mà chúng tôi đang sử dụng. Như chúng ta sẽ thảo luận sau trong bản thảo, các tập dữ liệu thiên vị, chẳng hạn như không bao gồm các tài liệu MS Office với mã VBA có thể dễ dàng thể hiện kết quả xuất sắc, nhưng trên thực tế, hoạt động khá kém. Ngược lại, tập dữ liệu của chúng tôi được tạo ra để ngăn chặn những thành kiến như vậy. Cái sau được sử dụng để xác định các tính năng không được sử dụng trong tài liệu, ví dụ:` VBA stomping, DDE, v.v. cho phép chúng tôi phát hiện các mẫu phần mềm độc hại khai thác, ví dụ: lỗ hổng MS Office mới`, không xác định tại thời điểm bài viết được gửi ban đầu . Phần sau minh họa rõ ràng hiệu quả và tiềm năng của phương pháp được đề xuất. Cuối cùng, các tính năng đã chọn và cách tiếp cận máy học rõ ràng vượt trội hơn so với công việc hiện có trong lĩnh vực này. Do các đặc điểm của tập dữ liệu này, để đảm bảo tính uy tín của kết quả của chúng tôi và để thúc đẩy nghiên cứu trong lĩnh vực này, tập dữ liệu được cung cấp trong Zenodo (Koutsokostas et al., 2021).
<br><br>
Trong phần tiếp theo, trước tiên chúng tôi cung cấp tổng quan về công việc liên quan đến phần mềm độc hại và tài liệu văn phòng. Cuối cùng, chúng tôi phân tích cấu trúc của chúng, các phương pháp được sử dụng để bọc giáp chúng, một số biện pháp đối phó và phương pháp phát hiện cũng như một số công cụ mã nguồn mở được sử dụng để xử lý các tệp MS Office. Sau đó, chúng tôi thảo luận về phương pháp của chúng tôi về thu thập, xử lý và phân tích dữ liệu. Sau đó, trong Phần 3, chúng tôi cung cấp tổng quan về tập dữ liệu của chúng tôi và một số phân tích khám phá về nội dung của nó. Phần 5 phân tích các phát hiện của chúng tôi và Phần 6 thảo luận về các phương pháp đã được xác định từ mỗi phần trong phương pháp của chúng tôi, cũng như hiệu quả và nhược điểm của chúng. Cuối cùng, bản thảo kết luận tóm tắt những phát hiện và đóng góp của chúng tôi.

## 2. Related work

Trong phần này, chúng tôi cung cấp cho người đọc những thông tin cơ bản cần thiết và tổng quan về công việc liên quan sẽ được sử dụng trong công việc của chúng tôi. Vì vậy, trước tiên chúng ta thảo luận về cấu trúc của các tài liệu được hỗ trợ bởi MS Office. Sau đó, chúng tôi trình bày các phương pháp cơ bản được đối thủ sử dụng để tạo lớp giáp cho tệp MS Office và các phương pháp được sử dụng để phát hiện các tài liệu đó. Cuối cùng, chúng tôi trình bày các công cụ nguồn mở nổi tiếng nhất được sử dụng để tạo các tài liệu MS Office độc hại hoặc để làm phong phú thêm khả năng của chúng.

### 2.1. Files supported by MS Office and their structure:

`MS Office hỗ trợ một số định dạng tài liệu; tuy nhiên, cốt lõi của hầu hết các tài liệu là XML.` Các định dạng này đã được giới thiệu khoảng hai thập kỷ trước trong Office XP để hỗ trợ định dạng dựa trên XML cho Excel và tiếp tục vào năm 2002 với một định dạng XML khác cho Word. Các định dạng này được tích hợp và trở thành định dạng mặc định cho MS Office 2003. Tuy nhiên, kể từ `MS Office 2007, MS đã áp dụng định dạng Office Open XML, còn được gọi là định dạng OpenXML hoặc OOXML, cho phép khả năng tương tác với các bộ và phần mềm xử lý tương tự khác. Định dạng Office Open XML dựa trên XML và đã được ECMA International áp dụng với tên gọi ECMA-376 (Ecma International, 2006) và trở thành tiêu chuẩn quốc tế (ISO / IEC 29500)` (International Organization for Standardization, 2016). Rõ ràng, về khả năng tương thích và khả năng tương tác, MS Office hỗ trợ tất cả các định dạng trước đó.
<br><br>
Các tệp theo định dạng OOXML là các gói chứa một số tệp XML cụ thể cùng với các tệp và tập lệnh đa phương tiện, được nén ở dạng tệp ZIP. Phần tử quan trọng của gói là tệp .xml [Content_Types] được lưu trữ trong thư mục gốc của gói. Tệp này đóng vai trò là chỉ mục của gói và liệt kê tất cả các kiểu nội dung của các phần mà gói chứa. Hai thư mục chính là _rels, lưu trữ các mối quan hệ giữa các phần khác và tài nguyên bên ngoài gói và docProps chứa các thuộc tính cốt lõi của tệp OOXML. Sau đó, tùy thuộc vào tệp, người ta có thể tìm thấy một thư mục word hoặc xl chứa nội dung của tệp. Trong các thư mục này, người ta thường có thể tìm thấy tệp vbaProject.bin; tài liệu tổng hợp OLE (Microsoft, 2020a) ở định dạng nhị phân, chứa mã VBA cho các macro mà OOXML có thể có. Cấu trúc điển hình của tài liệu OOXML và tệp bảng tính được minh họa ở bên trái và bên phải của Hình 1, tương ứng

![image](https://user-images.githubusercontent.com/62002485/164983031-17725416-a8f4-46e4-ba2c-876846017f8c.png)
<br><br>
Ngoài các định dạng trên, MS Office hỗ trợ các định dạng khác, trong đó quan trọng nhất từ góc độ bảo mật là Định dạng nhị phân tệp kết hợp (CFBF), là các tệp lưu trữ có cấu trúc (Microsoft, 2018b), tệp XLSB (Microsoft, 2021) cho MS Excel, có định dạng nhị phân và nhanh hơn đáng kể so với các tệp XLS / XLSX truyền thống.

### 2.2. Malicious MS Office files and their detection:

Không phải quá nhiều định dạng tệp khác nhau mà MS Office hỗ trợ đã tạo ra các vấn đề, mà thực tế là các định dạng này được thiết kế để hỗ trợ các tài liệu động. Tính năng động này được kích hoạt thông qua các thành phần và mô-đun khác nhau sử dụng nhiều phần lập trình. `Phần rõ ràng nhất là sự hỗ trợ cho VBA, cho phép ai đó viết mã tùy ý và thực thi nó bất cứ khi nào thấy cần thiết, thậm chí tự động khi tệp được mở hoặc đóng. Điều quan trọng là mã VBA không bị cô lập trong ngữ cảnh tài liệu MS Office, nhưng nó có thể tương tác với hệ thống tệp, trao đổi dữ liệu qua Internet và thực hiện các lệnh shell. Rõ ràng là những điều trên khiến người dùng gặp phải những rủi ro nghiêm trọng và đã được sử dụng rộng rãi trong phạm vi tấn công mạng trong nhiều năm. Ngoài VBA, người ta có thể thực hiện các tác vụ tương tự bằng cách sử dụng macro XLM (Microsoft, 2020b) và giao thức Trao đổi dữ liệu động (DDE)` (Microsoft, 2018a) hoặc sử dụng tài liệu XML thuần túy (Mendrez, 2015).
<br><br>
Một cuộc tấn công điển hình bằng cách sử dụng tài liệu MS Office được minh họa trong Hình 2. `Khi người dùng mở một tệp độc hại với MS Of fice, kẻ thù sẽ sử dụng trình kích hoạt để khởi chạy một tập lệnh VBA (ví dụ: orkbook_Open (), AutoOpen () và chức năng AutoClose ()) hoặc tự động đánh giá các giá trị động của tài liệu được liên kết với DDE (sử dụng ví dụ: Trình chỉnh sửa phương trình hoặc các phương trình ô) hoặc macro XLM được liên kết với ô. Mặc dù mã có thể thực hiện một số tác vụ, trong hầu hết các chiến dịch, ví dụ: Emotet (Patsakis và Chrysanthou, 2020), mã sẽ tải xuống một tải trọng từ Internet hoặc trích xuất nó từ chính tài liệu.` Tuy nhiên, điều này dẫn đến một vấn đề mới nhất cho đối thủ

![image](https://user-images.githubusercontent.com/62002485/164983314-e4c49732-3460-4c44-9166-5248bf684db2.png)


Các phiên bản Windows dưới dạng tệp đã tải xuống sẽ không được ký bởi cơ quan đáng tin cậy. Do đó, Kiểm soát tài khoản người dùng (UAC) của Windows sẽ yêu cầu sự đồng ý rõ ràng của người dùng để cho phép thực thi tệp. Vì người dùng rất có thể sẽ từ chối yêu cầu đó, các tài liệu độc hại sẽ tuân theo một cách tiếp cận khác. Thay vì thực hiện trực tiếp tải trọng độc hại, chúng sử dụng "proxy" được hệ điều hành tin cậy, ví dụ: PowerShell, Explorer. Có một số mã nhị phân và tập lệnh được cài đặt sẵn trong Windows hoặc được tải xuống bởi MS và được hệ điều hành ký hoặc đưa vào danh sách trắng, đồng thời cho phép thực hiện các chức năng bổ sung và có thể khai thác. Do chữ ký của họ, họ bỏ qua UAC của Windows, cho phép trong số những người khác thực thi mã tùy ý, biên dịch mã, cũng như tải xuống / tải lên tệp, xử lý kết xuất và thu thập thông tin đăng nhập mà không yêu cầu bất kỳ tương tác nào của người dùng. Các tệp nhị phân và tập lệnh này được gọi là Living Off The Land Binaries và Scripts (và cả Thư viện) hoặc LOLBAS (Campbell và cộng sự, 2020) và được sử dụng rộng rãi bởi các nhóm đỏ và phần mềm độc hại. Do đó, các tài liệu độc hại sử dụng LOLBAS để thực thi tải trọng độc hại và lây nhiễm sang máy chủ lưu trữ một cách liền mạch.
<br><br>
Để ngăn chặn sự phát hiện và phân tích của chúng, tài liệu MS Office có các macro của chúng được làm xáo trộn bằng cách sử dụng mã rác, các mã hóa khác nhau (chẳng hạn như base64, hex, octal), ngắt các chuỗi thành các chuỗi nhỏ hơn, kết quả của các hàm hoặc lạm dụng các chức năng liên quan đến MS Office. Các tính năng mã hóa vốn có . Trong những trường hợp này, kẻ thù sẽ gắn mật khẩu giải mã với email lừa đảo cho nạn nhân hoặc lạm dụng một lỗi Excel cũ (Lopera, 2020; Mendrez, 2020; Zhang, 2020). Cuối cùng, cần lưu ý rằng MS Office có một phiên bản đã biên dịch của mã VBA để đảm bảo tính mạnh mẽ và hiệu suất, được gọi là mã p.
Gần đây, tính năng này đang bị lạm dụng để thực thi mã độc. Chính xác hơn, trong cuộc tấn công này, được gọi là VBA stomping Vesselin Bontchev, (Harold Ogden và Kirk Sayre, 2018), kẻ thù sẽ xóa mã VBA khỏi tài liệu; tuy nhiên, MS Office thực hiện payload từ mã p được lưu trữ. Do đó, payload độc hại của mã VBA sẽ bị ẩn khỏi AV.
<br><br>
Để ngăn chặn việc thực thi mã VBA, các phiên bản mới nhất của MS Office yêu cầu người dùng cho phép thực thi thông qua một nút hiển thị “Bật nội dung”. Tuy nhiên, để phát hiện các tài liệu MS office độc ​​hại, hầu hết các nhà nghiên cứu đã nỗ lực sử dụng các phương pháp xử lý ngôn ngữ tự nhiên để trích xuất một số đặc điểm của mã VBA, cả về mã và về các chức năng được sử dụng Kim và cộng sự (2018); Mimura (2019); Mimura và Ohminami (2019). Phương pháp này là phát hiện một triệu chứng chứ không phải sự lây nhiễm, đó là sự hiện diện của mã bị xáo trộn trong macro VBA. Do đó, phạm vi là phát hiện sự mất cân bằng trong biểu diễn của các ký tự trong mã VBA đo entropy, độ dài của từ, tổng số ký tự trong mã và nhận xét, v.v. Về mặt mã, một số người trong số họ có xu hướng liệt kê các hàm gốc được sử dụng và nhóm chúng theo nội dung, chẳng hạn như văn bản, toán học, chuyển đổi, v.v. và sử dụng bộ phân loại học máy để xác định hiệu quả của nó
<br><br>
Tương tự, các nhà nghiên cứu sử dụng entropy n-gram (Bearden và Lo, 2017) và các thống kê cấp byte khác trên các đoạn luồng dữ liệu (Rudd và cộng sự, 2018) để phát hiện sự xáo trộn và do đó mô tả tệp là độc hại. Cuối cùng, (Nissim và cộng sự. , 2016) cũng đã khai thác cấu trúc phức tạp hơn của thư mục và tệp trong cấu trúc OOXML để phân loại tài liệu là độc hại.
<br><br>
Mặc dù thực tế là Microsoft đã vô hiệu hóa việc thực thi tự động mã nhúng, yêu cầu sự đồng ý của người dùng rõ ràng của người dùng để làm như vậy, như thực tế phổ biến đã cho thấy, điều này vẫn chưa giải quyết được vấn đề. Thật vậy, do sự phức tạp của tài liệu MS Office Các ví dụ nổi tiếng bao gồm việc sử dụng DDE (ví dụ: trong chiến dịch Hancitor malspam), macro XLM (ví dụ: được sử dụng trong chiến dịch malspam của Quakbot) hoặc gần đây trong chiến dịch ZLoader với việc sử dụng một tệp MS Office khác được tải xuống.
https://securityaffairs.co/wordpress/119902/hacking/malspam-new-evasion-technique-macro.html
<br>
Rõ ràng, hầu hết các phương pháp trên không thể được chống lại thông qua MS Office vì những kiểm tra bảo mật này nằm ngoài phạm vi của nó. Do đó, Microsoft đã tích hợp nhiều kiểm tra bảo mật như vậy trong Defender cho Endpoint2
https://www.microsoft.com/security/blog/2021/03/03/xlm-amsi-new-runtime-defense-against-excel-4-0-macro-malware/

### 2.3. Open-source VBscript and Office malicious generators:

Các phân tích gần đây (Ben Koehl & Joe Hannon, 2020; Jazi và Segura, 2020; Team, 2018) cho thấy rằng các công cụ mã nguồn mở, có thể xử lý các tài liệu MS Office do các nhà nghiên cứu tạo ra để tạo điều kiện cho các mô phỏng đối thủ nhóm đỏ, được sử dụng bởi các nhóm APT trong chiến dịch lừa đảo. Trong các đoạn sau, chúng tôi cung cấp phân tích ngắn gọn về các công cụ nguồn mở mạnh mẽ nhất có thể hỗ trợ quá trình thu thập tài liệu độc hại vì một số trong số chúng tập trung vào một khía cạnh duy nhất, ví dụ: làm mờ. Chúng tôi cung cấp tổng quan về các công cụ này trong Bảng 1.

![image](https://user-images.githubusercontent.com/62002485/164989757-9557cab2-ccc9-4b68-a346-d379a43dad4a.png)

- macro_pack Nasi là một trong những công cụ mạnh mẽ nhất trong quá trình tạo tài liệu độc hại và hỗ trợ rất nhiều kỹ thuật trốn tránh và đầu ra định dạng. Các phương thức thực thi của nó bao gồm WMI, Wscript, đối tượng COM, macro XLM, Task Scheduler, InvokeVerb, CreateProcess và Run PE. Nó hỗ trợ nhiều định dạng đầu ra khác nhau, từ định dạng Office đến định dạng VBScript như VBS, HTA, WSF, SCT và XSL. Nó cũng phục vụ một số lượng lớn các kỹ thuật trốn tránh bao gồm AV Bypass, VB và Command line Obfuscation, Tự giải mã trong bộ nhớ, Chạy trong bộ nhớ Excel, bỏ qua giao diện quét chống phần mềm độc hại (AMSI), thủ thuật Social Engineering, Anti sandbox, chạy tệp thực thi trong bộ nhớ, bỏ qua ASR, bỏ qua UAC và XLM Injection
- EvilClippy Hegt là một công cụ có thể được sử dụng để thao túng các tài liệu Office độc ​​hại hiện có để tránh phân tích tĩnh bởi công cụ chống vi-rút và công cụ phân tích macro VBA. Nó hoàn thành điều đó bằng cách triển khai một số kỹ thuật như ẩn macro VBA khỏi trình chỉnh sửa GUI, thực hiện VBA stomping, andomising tên mô-đun trong luồng thư mục hoặc khóa Dự án VBA.
- WePWNise Yiu có thể tạo mã VBA bổ sung mức độ thông minh để xác định điểm yếu và phân phối động tải trọng của nó, bỏ qua Chính sách hạn chế phần mềm (SRP) và mã nhị phân được bảo vệ trong Bộ công cụ giảm nhẹ nâng cao (EMET). Để đạt được điều này, nó liệt kê cài đặt đăng ký để xác định không được bảo vệ . nhị phân an toàn để đưa tải trọng qua các lệnh gọi WINAPI trong VBA.
- CACTUSTORCH (Jazi và Segura, 2020) là một khung thực thi shellcode sử dụng kỹ thuật DotNetToJScript để thực thi shellcode ở các định dạng kịch bản Windows như VBScript và Javascript. DotNetToJScript là một phương pháp để tải một cách phản xạ hợp ngữ a.NET sử dụng các ngôn ngữ kịch bản gốc của Windows.
- SharpShooter (Team, 2018) là một khuôn khổ tạo tải trọng.
Trong số nhiều khả năng của nó, nó cho phép tạo các tải trọng ở nhiều định dạng khác nhau như HTA, JS, VBS, WSF. Nó cũng có thể tạo các tài liệu hỗ trợ Excel 4.0 SLK Macro và thực thi mã C # tùy ý từ một tập lệnh VB. Ví dụ: nó có thể tạo một tệp VBS thực thi Mimikatz. Nó cũng có thể kết hợp phân tích bỏ qua AMSI và chống hộp cát trong số nhiều kỹ thuật của nó.
- EXCELntDonut Security là một công cụ có thể xuất macro XLM từ tệp nguồn C # và cũng áp dụng kiểm tra hộp cát và giải mã.
- Macrome Weber là một công cụ cũng có thể tạo các tài liệu XLM Macro (macro Excel 4.0) từ đầu vào shellcode cũng như áp dụng mức độ xáo trộn
- LuckyStrike Lang là một trình tạo tài liệu độc hại mạnh mẽ khác có thể nhúng các lệnh shell tiêu chuẩn, tập lệnh PowerShell tùy chỉnh hoặc thậm chí các tệp thực thi (.exe) làm trọng tải. Hơn nữa, nó sử dụng Invoke-Obfuscation để làm xáo trộn tải trọng. Nó lây nhiễm tài liệu theo nhiều cách khác nhau , bao gồm Wscript.Shell để kích hoạt lệnh trong cửa sổ ẩn, nhúng ô của tập lệnh PowerShell được mã hóa hoặc mã hóa base64. lưu dưới dạng tệp văn bản vào đĩa và sử dụng certutil để giải mã và thực thi lệnh đó hoặc chèn tải trọng vào siêu dữ liệu của tệp độc hại trong trường Chủ đề. Nó cũng có thể sử dụng Invoke-ReflectivePEInjection để thực thi một PE được mã hóa b64 được đưa vào đĩa hoặc đưa nó vào một quy trình khác

## 3. Methodology:
Để thực hiện phân tích chính xác tình trạng hiện đại trong các tài liệu văn phòng độc hại, chúng ta cần thiết lập một phương pháp luận chặt chẽ cho các nhiệm vụ phải thực hiện và đánh giá kết quả mong đợi từ mỗi bước. Nói chung, phương pháp luận của chúng tôi bao gồm ba giai đoạn, mỗi giai đoạn có các nhiệm vụ cụ thể được thực hiện. Sơ lược về phương pháp luận của chúng tôi được sử dụng để phân tích trong công trình này được minh họa trong Hình 3. Phương pháp luận bao gồm ba bước chính được phân tích trong các đoạn sau:

![image](https://user-images.githubusercontent.com/62002485/164990388-86250e60-ec16-4905-b487-1edb1943ba37.png)


- Thu thập dữ liệu Trong bước này, chúng tôi đã thu thập tất cả dữ liệu có sẵn từ các nguồn công khai. Chúng tôi muốn làm nổi bật bản chất công khai của các nguồn tài liệu ở đây vì việc sử dụng các nguồn tư nhân ngụ ý một số vấn đề về phương pháp luận. Rõ ràng nhất là khía cạnh pháp lý khi các tổ chức khá ngần ngại trong việc chia sẻ tài liệu. Ngay cả khi các tài liệu độc hại, vì chúng có thể là sản phẩm của một cuộc tấn công phishing, ví dụ: các tài liệu nội bộ bị chặn có thể đã được sử dụng, họ không chia sẻ chúng. Tuy nhiên, bằng cách tìm thấy chúng trong các trang web công cộng và / hoặc kho chứa phần mềm độc hại công khai, trên thực tế, người nhận đã đồng ý sử dụng chúng, giúp giảm bớt bất kỳ vấn đề pháp lý nào đối với việc thu thập và xử lý. Hơn nữa, các nguồn công khai không thiên vị hơn vì trái ngược với các nguồn riêng tư, chúng chứa các mẫu từ nhiều chiến dịch hơn. Trong trường hợp là các mẫu lành tính, chúng tôi thu thập dữ liệu ngẫu nhiên các trang web công cộng chính thức khác nhau, chính xác hơn là tổng số 726 trang web chính phủ và 1010 trang web giáo dục thuộc các quốc gia và tổ chức khác nhau, với một đường dẫn tự động truy vấn tài liệu Google cho MS Office với xls, xlsx, doc và phần mở rộng docx trong các trang web như vậy. Các tệp được truy xuất đã được kiểm tra để xác định xem chúng có bao gồm macro hay không và tệp sau đã bị loại bỏ. Sau đó, các tệp này được gửi tới Triage và VirusTotal để xác nhận rằng chúng không độc hại. Sau những thao tác này, tổng số 2736 tệp lành tính đã được chọn cho tập dữ liệu của chúng tôi. Rõ ràng, các tệp lành tính nói trên gây ra khuynh hướng chống lại phương pháp luận của chúng tôi vì các tệp điển hình sẽ không đáp ứng các yêu cầu này, tuy nhiên, đó là cách tiếp cận tốt nhất để nhấn mạnh tính hiệu quả của phương pháp tiếp cận của chúng tôi. Do đó, cơ sở dữ liệu của chúng tôi được tạo ra để minh họa cho trường hợp xấu nhất, trong đó chúng tôi cố gắng phân biệt các tệp độc hại với các tệp lành tính, trong cả hai trường hợp sử dụng macro và DDE. Lưu ý rằng nếu tệp không chứa macro (VBA và DDE ẩn được phát hiện bởi các tính năng của chúng tôi, như được mô tả sau trong Phần 5), thì tệp có thể được phân loại là lành tính mà không cần tính thêm các tính năng, do đó giúp giảm bớt nhiệm vụ phân loại.
<br>Trong trường hợp tệp độc hại, chúng tôi đã sử dụng ba kho lưu trữ nổi tiếng, đó là AppAny, Virusign và Malshare để thu thập các mẫu của chúng tôi. Hơn nữa, chúng tôi đã sử dụng VirusTotal để thu thập thêm một số thông tin về mẫu của chúng tôi. Chính xác hơn, chúng tôi đã thu thập thông tin về tỷ lệ phát hiện của các sản phẩm chống vi-rút khác nhau trên các tài liệu này cũng như thông tin bổ sung về các lệnh gọi DNS mà các mẫu này đã thực hiện.
- Phân tích tài liệu Trong giai đoạn thứ hai, chúng tôi phân tích tài liệu của chúng tôi theo cách tĩnh và động. Trong phân tích tĩnh của chúng tôi, chúng tôi đã trích xuất tất cả siêu dữ liệu có thể sử dụng oletools Lagadec và ExifTool Harvey, sau đó trích xuất mã VBA bằng oletools. Trong phân tích động của chúng tôi, chúng tôi đã thực thi tất cả các mẫu trong môi trường hộp cát thu thập tất cả các hành động PowerShell, tất cả các quy trình đã được mở và tất cả các yêu cầu DNS đã được thực hiện. Hơn nữa, đối với mỗi tập lệnh PowerShell được thu thập, chúng tôi đã cố gắng thực hiện giải mã tự động
- Trích xuất tính năng & phân tích tổng hợp Tất cả thông tin thu thập nói trên được lưu trữ ở định dạng JSON để tạo điều kiện xử lý cho giai đoạn cuối cùng. Trong giai đoạn cuối cùng này, chúng tôi đã thực hiện phân tích kỹ lưỡng các kết quả, cố gắng trích xuất một số đặc điểm hành vi và tĩnh mà mẫu của chúng tôi thể hiện. Ví dụ: chúng tôi muốn xác định các phương pháp và hành động được sử dụng phổ biến nhất được sử dụng trong VBA và PowerShell từ các tài liệu này. Hơn nữa, chúng tôi muốn xác định xem liệu các tài liệu có đang sử dụng các khai thác cụ thể hay không. Cuối cùng, chúng tôi đã khám phá các mối quan hệ mà các mẫu này có liên quan đến các lệnh gọi DNS mà chúng đã thực hiện.

## 4. Dataset exploration:

Ngoài 2736 mẫu lành tính được thu thập theo phương pháp được báo cáo trong Phần 3, bộ dữ liệu của chúng tôi bao gồm 15571 mẫu độc hại có từ năm 2006 đến năm 2020 theo ngày ghi được thu thập thông qua VirusTotal, AppAny, Virusign và Malshare. Theo tên gốc tương ứng của chúng, mẫu bao gồm 13518 tài liệu Word, 1996 Excel
bảng tính và 13 bài thuyết trình Powerpoint. Trong VirusTotal, 15571 đã được quét với 14264 tệp trong số này được báo cáo là độc hại bởi ít nhất một phần mềm chống vi-rút (AV). Lưu ý rằng không có tệp nào được chia sẻ chính thức được báo cáo là độc hại. Tuy nhiên, các báo cáo cho thấy sự khác biệt lớn về số lượng antivirus phát hiện ra chúng và loại vi rút nào tại mỗi thời điểm. Tuy nhiên, cần lưu ý rằng các kết quả này đề cập đến lần quét cuối cùng chứ không phải lần quét đầu tiên, vì trung bình mỗi tài liệu đã được quét 9,39 lần sau khi người dùng gửi. Thực tế, điều này có nghĩa là một số AV liên tục không gắn cờ chúng là độc hại. Như được minh họa trong Hình 4, có rất nhiều khác biệt trong các kết quả được báo cáo của các AV. Tuy nhiên, những kết quả này có thể là do chữ ký được chia sẻ hoặc các cách tiếp cận khác nhau, ví dụ: hành vi thời gian chạy. Nói chung, vì người dùng có thể gửi tệp tới VirusTotal để quét, nên hiển nhiên là họ sẽ nhận được nhiều báo cáo với các kết quả khác nhau tùy theo độ chính xác của kết quả được báo cáo của tất cả các AV tại thời điểm quét.
<br><br>
Về siêu dữ liệu, do có nhiều định dạng khác nhau, chúng tôi trình bày thống kê tương ứng trong Bảng 2. Rõ ràng là trong khi có nhiều tài liệu mà siêu dữ liệu và nội dung chưa được quan tâm, ví dụ: chúng không có bất kỳ nội dung nào bên trong, có số lượng chỉnh sửa thấp, v.v., trong hầu hết các trường hợp, tác giả phần mềm độc hại đã thực hiện các biện pháp để hiển thị một số hành động có liên quan và làm cho chúng thực tế hơn. Từ các mẫu, 15.021 mã VBA nhúng được trích xuất bằng các oletools. Các thống kê và tính năng khác nhau được thu thập từ các tệp này được mô tả trong Bảng 3. Về vấn đề này, chúng tôi báo cáo kích thước của mã VBA, khi mã VBA được thực thi, lệnh shell đã được sử dụng bao nhiêu lần, bao nhiêu mẫu đã sử dụng chuỗi Base64, hoặc có bao nhiêu người cố gắng ghi dữ liệu vào một tệp. Hơn nữa, trong Hình 5, chúng tôi trình bày một đoạn của đồ thị minh họa sự liên kết giữa các mẫu và các miền mà chúng đã cố gắng kết nối với nhau. Đáng chú ý, như được minh họa trong Hình 6, hầu hết các miền được xác định đều được VirusTotal coi là vô hại. Điều thứ hai có thể được cho là do kết nối với một số miền thực sự lành tính để thu thập dữ liệu, thông tin xác thực, v.v. hoặc thậm chí là các miền bị xâm phạm tạm thời nơi các tệp thực thi độc hại có thể đã được đưa vào. Ví dụ: pastebin không phải là một dịch vụ độc hại nhưng thường được sử dụng để kết xuất thông tin xác thực trong khi các dịch vụ của Google, vì chúng được đưa vào danh sách trắng nên chúng thường bị khai thác hoặc sử dụng để lấy thông tin nhạy cảm của người dùng.
<br><br>
Ngoài phân tích trước đó, chúng tôi đã thu thập các lệnh gọi hệ thống được thực hiện bởi Living Off The Land Binaries từ các tập lệnh PowerShell có trong tệp tập dữ liệu trong Bảng 4 và phân tích PSDecode3 (một công cụ giải mã cho PowerShell) trong Bảng 5. Như nó có thể được quan sát từ cả hai bảng, CMD.EXE xuất hiện nhiều lần vì nhiệm vụ thực tế là tải xuống tệp từ Internet và thực thi tệp. Lưu ý rằng việc sử dụng LOLBAS ngụ ý rằng sẽ không có cảnh báo nào được đưa ra cho người dùng.

![image](https://user-images.githubusercontent.com/62002485/164990808-afcbbec9-ea9a-4791-b5c7-e650279b7afc.png)

![image](https://user-images.githubusercontent.com/62002485/164990871-45868390-05c3-44bd-9628-e2293eaba835.png)

![image](https://user-images.githubusercontent.com/62002485/164990880-0cd33f2f-f036-47da-aea9-b4fa7b047701.png)

## 5. Experiments:

Với các mẫu được thu thập bằng cách sử dụng phương pháp được trình bày trong Phần 3, chúng tôi đã tính toán một tập hợp các tính năng nhị phân được mô tả trong Bảng 6. Chúng tôi đánh giá sức mạnh của các tính năng được đề xuất của chúng tôi để phân biệt giữa các mẫu độc hại và lành tính (tức là phân loại nhị phân). Chúng tôi đã chọn một tập hợp các phương pháp học máy để tận dụng nhiệm vụ phân loại nhị phân. Cụ thể hơn, chúng tôi đã sử dụng Random Forest, một bộ phân loại tập hợp không tham số, XGBoost; triển khai cây quyết định được tăng cường độ dốc, Bộ phân loại vectơ hỗ trợ (SVC) và bộ phân loại Perceptron (MLP) nhiều lớp

![image](https://user-images.githubusercontent.com/62002485/164991038-5a885af4-4a7c-4ffd-90f6-33745f57d37e.png)
<br>
Các siêu tham số của mỗi mô hình được điều chỉnh bằng tìm kiếm lưới để tối đa hóa hiệu suất phân loại trong nhiệm vụ phân biệt giữa các mẫu lành tính và độc hại. Bảng 7 tóm tắt các thông số cấu hình đạt được hiệu suất cao nhất. Trong trường hợp của cả Random Forest và XGBoost, chúng tôi nhận thấy rằng số lượng tối đa các tính năng được sử dụng cho nhiệm vụ phân loại thấp hơn nhiều so với tổng số các tính năng được tính cho mỗi tệp (tức là chúng tôi đã tính toán 40 tính năng khác nhau, xem Bảng 6), sẽ được thảo luận sau trong phần này. Trong trường hợp của mô hình SVC, chúng tôi đã sử dụng hạt nhân hàm cơ sở xuyên tâm (rbf). Trong tất cả các thử nghiệm, chúng tôi sử dụng xác thực chéo 10 lần tiêu chuẩn và lặp lại thí nghiệm đó ba lần để có được ước tính gần như không thiên vị về hiệu suất của các mô hình dự đoán mà chúng tôi đã đào tạo.

![image](https://user-images.githubusercontent.com/62002485/164991153-51f14be1-7923-4694-9589-83efa7e23164.png)
<br><br>
Tất cả các thử nghiệm của chúng tôi đều được thực hiện trên một hệ thống được trang bị NVIDIA TITAN Xp PG611-c00 để tăng tốc độ tính toán, trong khi chúng tôi sử dụng các triển khai của thư viện scikit-learning4. Chúng tôi đánh giá hiệu suất của các bộ phân loại được đào tạo bằng cách sử dụng các chỉ số phân loại tiêu chuẩn về độ chính xác, độ thu hồi, độ chính xác và điểm F1. Kết quả đạt được của mỗi mô hình được tóm tắt trong Bảng 8. Có thể thấy, kết quả thu được khi xem xét các chỉ số phân loại tiêu chuẩn cho tất cả các bộ phân loại là gần 100%, với Random Forest và MLP thể hiện hiệu suất tốt nhất so với hai loại còn lại các mô hình. Hơn nữa, các giá trị thấp của độ lệch chuẩn cho tất cả các bộ phân loại cho thấy mức độ mạnh mẽ của các kết quả thử nghiệm
<br><br>
Theo Bảng 7 và 8, chúng tôi nhận thấy rằng kết quả tốt nhất đạt được khi chỉ sử dụng một tập hợp con các tính năng của hệ thống của chúng tôi. Do đó, chúng tôi đã nghiên cứu mức độ liên quan của các tính năng trong Random Forest, XGBoost và mô hình SVC, được mô tả trong Hình 7. Như có thể quan sát thấy, một tập hợp con chung các tính năng có mức độ liên quan đáng kể trong cả ba mô hình. Đặc biệt, chúng tôi nhận thấy rằng autopen, shell, createobject, base64 string và document_open là những tính năng phù hợp nhất trong cả ba mô hình

![image](https://user-images.githubusercontent.com/62002485/164991213-841875ff-0f27-43bc-a7f6-1e9b08d24a8e.png)

![image](https://user-images.githubusercontent.com/62002485/164991219-21dfa4ad-fcbb-413d-84d6-3f64546affba.png)

![image](https://user-images.githubusercontent.com/62002485/164991233-a1d5ceb0-a2ce-4656-b860-56b5ca89a2a6.png)







