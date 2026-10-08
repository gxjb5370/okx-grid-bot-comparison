# bot lưới OKX: Hướng dẫn chọn Spot Grid hay Futures Grid, tính phí và bắt đầu tự động hóa giao dịch

Khi tìm **bot lưới OKX**, phần lớn người dùng không chỉ muốn biết “bot này là gì”. Họ thường muốn giải đáp nhanh hơn:

- Bot lưới có thật sự phù hợp với cách giao dịch của mình không?
- Nên dùng **Spot Grid** hay **Futures Grid**?
- Cài khoảng giá và số lượng lưới thế nào?
- Phí giao dịch có làm mất phần lợi nhuận nhỏ của từng lệnh không?
- Bot có tự dừng khi giá phá biên độ không?
- Dùng mã mời `CASH20` có giúp giảm chi phí giao dịch không?

Bài viết này tập trung vào những câu hỏi đó. OKX hiện cung cấp hai loại bot lưới chính là **Spot Grid** và **Futures Grid**. Spot Grid mua và bán tài sản trên thị trường giao ngay, còn Futures Grid giao dịch hợp đồng tương lai với các chế độ Long, Short và Neutral.

> Bot lưới không phải máy in tiền tự động. Nó chỉ thực hiện quy tắc mua thấp, bán cao hoặc mở và đóng vị thế theo các mức giá mà bạn cài đặt. Nếu thị trường đi một mạch ra khỏi vùng giá, bot có thể giữ tài sản, chịu lỗ thả nổi hoặc bị thanh lý trong trường hợp dùng futures có đòn bẩy.

## Bot lưới OKX hoạt động như thế nào?

Bot lưới chia một khoảng giá thành nhiều mức nhỏ. Mỗi mức có thể được xem như một “bậc thang” để bot đặt lệnh.

Ví dụ, bạn cài một bot Spot Grid cho BTC/USDT trong vùng **80.000–90.000 USDT** với 10 lưới. Bot sẽ phân bổ lệnh mua và bán quanh vùng giá đó. Khi giá giảm xuống một mức lưới, bot có thể mua. Khi giá tăng lên mức lưới kế tiếp, bot bán ra. Sau khi một cặp lệnh hoàn tất, hệ thống tiếp tục đặt lệnh mới để lặp lại chiến lược. Cơ chế thực tế còn phụ thuộc vào loại tiền dùng để đầu tư, cách khởi tạo lệnh và điều kiện thị trường.

Điểm cốt lõi của chiến lược này là **biến động trong một vùng giá**, không phải dự đoán chính xác giá sẽ tăng hay giảm trong dài hạn. Nếu giá liên tục dao động lên xuống, bot có nhiều cơ hội hoàn thành các chu kỳ mua và bán. Nếu giá tăng hoặc giảm quá mạnh theo một hướng, nhiều lệnh có thể chỉ được khớp một chiều.

### Ví dụ đơn giản

Giả sử bạn đặt vùng giá từ 100 đến 110 USDT với 5 lưới:

- Lưới 1: 100–102
- Lưới 2: 102–104
- Lưới 3: 104–106
- Lưới 4: 106–108
- Lưới 5: 108–110

Khi giá giảm từ 106 xuống 104, bot có thể mua ở vùng thấp hơn. Nếu sau đó giá quay lại 106, bot bán theo mức lưới đã cài. Phần chênh lệch là lợi nhuận gộp của chu kỳ, trước khi trừ phí giao dịch.

Lợi nhuận thực tế của mỗi chu kỳ luôn phải tính đến:

- Phí mua và phí bán.
- Chênh lệch giữa giá lý thuyết và giá khớp thực tế.
- Khối lượng của mỗi lưới.
- Vốn đang nằm trong tài sản hoặc stablecoin.
- Funding fee nếu dùng Futures Grid.
- Lãi hoặc lỗ chưa thực hiện của các vị thế còn mở.

## Các loại bot lưới trên OKX

OKX hiện mô tả hai nhóm bot lưới chính phục vụ nhu cầu này: **Spot Grid** và **Futures Grid**. Nền tảng cũng có các bot khác như DCA, Smart Arbitrage, Recurring Buy, Signal Bot và TWAP, nhưng chúng không phải bot lưới theo nghĩa thông thường.

### Spot Grid

Spot Grid giao dịch tài sản thật trên thị trường spot. Nếu bạn tạo bot BTC/USDT, bot sẽ sử dụng USDT để mua BTC ở các mức thấp hơn và bán BTC ở các mức cao hơn trong phạm vi đã đặt.

OKX cho phép người dùng chọn cách nạp vốn bằng:

- Tiền định giá của cặp giao dịch, chẳng hạn USDT.
- Tài sản cơ sở, chẳng hạn BTC.
- Kết hợp cả tài sản cơ sở và tiền định giá.
- Stablecoin trong một số thị trường đủ điều kiện.

Cách chọn vốn ảnh hưởng đến các lệnh được đặt ngay khi bot khởi chạy. Nếu chỉ nạp USDT, bot thường ưu tiên tạo các lệnh mua. Nếu chỉ nạp BTC, bot có thể tạo các lệnh bán. Nếu dùng cả hai loại tài sản, hệ thống phân bổ lệnh theo số vốn và trạng thái giá hiện tại.

OKX hiện cho biết Spot Grid có thể hỗ trợ tối đa **1.000 lưới**, cao hơn giới hạn trước đây là 300 lưới. Nền tảng cũng cung cấp tùy chọn thiết lập thủ công, tham số được đề xuất từ chiến lược AI và chức năng tích hợp Simple Earn cho phần vốn nằm ngoài vùng giao dịch trong một số trường hợp.

Spot Grid thường dễ hiểu hơn với người mới vì không có đòn bẩy và không phát sinh nguy cơ thanh lý theo cơ chế futures. Tuy vậy, điều đó không có nghĩa là không có rủi ro. Nếu giá giảm mạnh và nằm dưới biên dưới trong thời gian dài, bot có thể giữ phần lớn vốn dưới dạng tài sản đang giảm giá.

### Futures Grid

Futures Grid dùng hợp đồng tương lai thay vì mua bán trực tiếp tài sản. Điểm khác biệt lớn là bạn có thể chọn hướng giao dịch và sử dụng đòn bẩy.

OKX hiện cung cấp ba chế độ chính:

- **Long**: phù hợp với kịch bản kỳ vọng giá dao động và có xu hướng tăng.
- **Short**: phù hợp với kịch bản kỳ vọng giá dao động và có xu hướng giảm.
- **Neutral**: đặt lệnh mua bên dưới và lệnh bán bên trên giá hiện tại, tìm cơ hội từ biến động hai chiều.

Ở chế độ Neutral, bot có thể mở vị thế long khi giá giảm xuống vùng dưới và mở vị thế short khi giá tăng lên vùng trên. Một chu kỳ hoàn chỉnh có thể là mua rồi bán, hoặc bán rồi mua, tùy theo hướng biến động và trạng thái vị thế.

Futures Grid linh hoạt hơn Spot Grid nhưng khó kiểm soát rủi ro hơn. Giá đi ngược hướng, đòn bẩy cao, funding fee tích lũy hoặc biên độ đặt quá hẹp đều có thể khiến kết quả xấu đi nhanh chóng. OKX cũng nêu rõ một bot futures có thể dừng do thanh lý, cặp giao dịch bị hủy niêm yết, giới hạn vị thế, thay đổi thông số hợp đồng hoặc các biện pháp kiểm soát rủi ro khác.

## So sánh đầy đủ các lựa chọn bot lưới

OKX không bán bot lưới theo kiểu các gói Basic, Pro hoặc Enterprise có phí thuê bao cố định. Chi phí chính đến từ giao dịch phát sinh, trong khi tài khoản có thể chịu mức phí khác nhau tùy khu vực, sản phẩm, cấp độ người dùng và khối lượng giao dịch 30 ngày.

Bảng dưới đây bao quát các sản phẩm và chế độ bot lưới công khai liên quan trực tiếp đến từ khóa **bot lưới OKX**:

| Sản phẩm / chế độ | Cách hoạt động | Đòn bẩy | Phí / giá sử dụng | Chu kỳ tính phí | Phù hợp với | Liên kết |
| --- | --- | ---: | --- | --- | --- | --- |
| Spot Grid | Tự động mua thấp và bán cao trong vùng giá đã chọn | Không | Không có phí thuê bot; áp dụng phí giao dịch Spot hiện hành | Tính theo từng lệnh khớp | Người mới, giao dịch tài sản thật, thị trường đi ngang | [ Đăng ký OKX và mở Spot Grid](https://okx.com/join/CASH20) |
| Futures Grid Long | Mở và đóng vị thế long theo các mức lưới | Có thể dùng | Không có phí thuê bot; áp dụng phí futures, funding và chi phí liên quan nếu có | Tính theo từng lệnh và kỳ funding | Người có nhận định nghiêng về tăng và hiểu futures | [ Bắt đầu Futures Grid Long trên OKX](https://okx.com/join/CASH20) |
| Futures Grid Short | Mở và đóng vị thế short theo các mức lưới | Có thể dùng | Không có phí thuê bot; áp dụng phí futures, funding và chi phí liên quan nếu có | Tính theo từng lệnh và kỳ funding | Người có nhận định nghiêng về giảm và hiểu futures | [ Bắt đầu Futures Grid Short trên OKX](https://okx.com/join/CASH20) |
| Futures Grid Neutral | Đặt lệnh hai chiều quanh giá hiện tại | Có thể dùng | Không có phí thuê bot; áp dụng phí futures, funding và chi phí liên quan nếu có | Tính theo từng lệnh và kỳ funding | Thị trường biến động hai chiều trong vùng tương đối rõ | [ Khám phá Futures Grid Neutral](https://okx.com/join/CASH20) |

Bảng phí thực tế cần được kiểm tra trực tiếp trên tài khoản và cặp giao dịch trước khi tạo bot. OKX cho biết phí maker và taker được hiển thị trong bảng đặt lệnh, đồng thời phí có thể khác giữa thị trường spot và futures.

## Phí giao dịch ảnh hưởng đến bot lưới ra sao?

Bot lưới thường kiếm phần chênh lệch nhỏ trên mỗi chu kỳ. Vì vậy, phí giao dịch có thể là yếu tố quyết định một chiến lược nhìn có vẻ sinh lời trên màn hình có thực sự có lãi sau cùng hay không.

Công thức đơn giản:

text
Lợi nhuận ròng mỗi chu kỳ
= Chênh lệch giá mua và bán
- Phí lệnh mua
- Phí lệnh bán
- Các chi phí liên quan khác


Ví dụ, nếu khoảng cách giữa hai lưới quá nhỏ, lợi nhuận gộp của một chu kỳ có thể không đủ bù hai lần phí giao dịch. Tình huống này đặc biệt đáng chú ý khi bot hoạt động với số lượng lưới rất lớn hoặc liên tục khớp các lệnh nhỏ.

Với Futures Grid, phần tính toán còn phức tạp hơn vì có thể bao gồm:

- Funding fee.
- Lãi hoặc lỗ chưa thực hiện.
- Phí taker nếu lệnh được khớp ngay thay vì nằm chờ trên sổ lệnh.
- Chi phí liên quan đến việc tăng hoặc giảm margin.
- Phí thanh lý nếu vị thế bị đóng cưỡng bức.

OKX phân biệt **Grid Profit** và **Unpaired PnL**. Grid Profit chỉ tính những chu kỳ mua-bán hoặc bán-mua đã hoàn tất. Unpaired PnL có thể bao gồm lãi lỗ thả nổi, funding fee và một phần phí giao dịch chưa được gán vào chu kỳ hoàn chỉnh.

Đây là lý do không nên chỉ nhìn vào con số “Grid Profit”. Khi đánh giá một bot đang chạy, hãy xem cả:

- Total PnL.
- Grid Profit.
- Unpaired PnL.
- Vị thế hiện đang mở.
- Funding fee.
- Giá thanh lý ước tính nếu là futures.
- Tỷ lệ lợi nhuận sau phí.

## Mã mời `CASH20` có tác dụng gì?

Liên kết được cung cấp cho bài viết này là liên kết đăng ký OKX với mã mời `CASH20`. Theo thông tin đi kèm, mã được quảng bá với mức **hoàn hoặc giảm 20% phí giao dịch**.

OKX hiện mô tả chương trình giới thiệu có thể áp dụng mức giảm cho người được mời trong khoảng **0–20%**, tùy mã mời, chương trình, khu vực và điều kiện tài khoản. Mức giảm không nên được hiểu là giảm trực tiếp vào giá tài sản hay bảo đảm lợi nhuận từ bot.

Khi đăng ký, nên kiểm tra ba điểm:

1. Mã `CASH20` đã được điền sẵn hoặc hiển thị đúng trong quy trình đăng ký hay chưa.
2. Tài khoản của bạn có thuộc khu vực và nhóm người dùng đủ điều kiện hay không.
3. Mức giảm thực tế được hiển thị trong phần phí hoặc chương trình giới thiệu sau khi hoàn tất đăng ký.

Nếu bạn đã có tài khoản OKX, việc bổ sung mã sau khi tài khoản được tạo có thể không được chấp nhận. Vì vậy, hãy kiểm tra mã ngay trong quá trình đăng ký thay vì chờ đến khi tạo bot.

> Giảm phí không biến một chiến lược xấu thành chiến lược tốt. Nó chỉ làm giảm một phần chi phí giao dịch. Khoảng giá, số lượng lưới, thanh khoản và cách kiểm soát rủi ro vẫn quan trọng hơn.

## Cách tạo Spot Grid trên OKX

Quy trình có thể thay đổi đôi chút giữa ứng dụng và phiên bản web, nhưng logic chung gồm các bước sau:

### 1. Mở khu vực Trading Bots

Trong OKX, truy cập khu vực **Trade** rồi chọn **Trading Bots** hoặc **Grid Bots**. OKX cung cấp cả lựa chọn tạo bot thủ công, tham số được đề xuất và bot do trader khác công khai trên Marketplace trong những thị trường đủ điều kiện.

### 2. Chọn cặp giao dịch

Người mới thường bắt đầu bằng cặp có thanh khoản cao vì chênh lệch giá và khả năng khớp lệnh dễ dự đoán hơn so với các token ít giao dịch. Tuy nhiên, thanh khoản không loại bỏ rủi ro biến động. Một cặp lớn vẫn có thể phá biên độ khi thị trường có tin tức mạnh.

### 3. Chọn khoảng giá

Biên dưới và biên trên xác định vùng mà bot sẽ hoạt động.

Khoảng quá hẹp có thể làm bot dừng hoạt động sớm khi giá đi ra ngoài vùng. Khoảng quá rộng lại khiến số vốn được phân bổ cho từng lưới nhỏ hơn, làm lợi nhuận mỗi chu kỳ thấp hơn.

Một cách tiếp cận thực tế là xem xét:

- Giá thấp và cao trong một khoảng thời gian phù hợp.
- Vùng hỗ trợ và kháng cự gần đây.
- Khả năng thị trường tiếp tục đi ngang hay đang hình thành xu hướng mạnh.
- Khoảng cách giữa giá hiện tại và hai biên.
- Mức lỗ bạn có thể chấp nhận nếu giá rời khỏi vùng.

### 4. Chọn số lượng lưới

Nhiều lưới hơn không đồng nghĩa với lợi nhuận cao hơn.

Số lượng lưới lớn làm khoảng cách giữa các mức nhỏ đi. Điều này có thể tạo nhiều chu kỳ hơn, nhưng lợi nhuận gộp cho mỗi chu kỳ cũng giảm. Nếu khoảng cách nhỏ hơn tổng chi phí giao dịch, bot có thể hoạt động rất nhiều nhưng kết quả ròng không đáng kể.

Ngược lại, quá ít lưới khiến khoảng cách giữa các mức lớn. Bot có thể ít giao dịch hơn và bỏ qua một số dao động nhỏ.

### 5. Chọn cách phân chia

OKX hỗ trợ cách chia lưới theo kiểu **Arithmetic** hoặc **Geometric** trong các bot lưới phù hợp.

- Arithmetic chia theo khoảng chênh lệch giá bằng nhau.
- Geometric chia theo tỷ lệ phần trăm tương đối giữa các mức.

Arithmetic dễ hình dung khi bạn muốn mỗi lưới có cùng chênh lệch giá tuyệt đối. Geometric thường phù hợp hơn khi bạn muốn mỗi bậc phản ánh tỷ lệ phần trăm tương tự nhau, đặc biệt khi khoảng giá rộng.

### 6. Kiểm tra vốn và lợi nhuận ước tính

Trước khi xác nhận, hãy kiểm tra số vốn trên mỗi lưới. Đừng chỉ nhìn vào lợi nhuận ước tính do hệ thống hiển thị. Đây thường là kết quả mô phỏng dựa trên dữ liệu hoặc thông số nhất định, không phải cam kết trong tương lai.

Sau khi tạo bot, vốn được phân bổ cho chiến lược và không còn hoạt động như số dư giao dịch thông thường cho đến khi bạn dừng hoặc điều chỉnh bot theo quy định của sản phẩm.

## Cách tạo Futures Grid an toàn hơn

Futures Grid dành cho người đã hiểu các khái niệm như margin, funding fee, liquidation price và vị thế long/short.

Khi thiết lập, bạn thường phải chọn:

- Hợp đồng giao dịch.
- Chế độ Long, Short hoặc Neutral.
- Biên dưới và biên trên.
- Số lượng lưới.
- Kiểu Arithmetic hoặc Geometric.
- Số vốn.
- Đòn bẩy.
- Take-profit và stop-loss.
- Phần margin dự phòng.

OKX cho biết người dùng có thể điều chỉnh khoảng giá và số lượng lưới trong khi bot đang chạy trong một số trường hợp. Tuy nhiên, việc chỉnh tham số có thể ảnh hưởng đến trailing settings, cách tái đầu tư PnL và yêu cầu bổ sung tài sản định giá.

### Long hay Short?

Không nên chọn Long chỉ vì thị trường vừa tăng, cũng không nên chọn Short chỉ vì giá vừa giảm. Bot lưới hoạt động dựa trên hành vi giá sau khi khởi tạo, còn dữ liệu quá khứ không bảo đảm thị trường sẽ lặp lại.

- Long phù hợp hơn khi bạn chấp nhận giữ vị thế mua và cho rằng giá có thể dao động trong vùng với xu hướng tăng.
- Short phù hợp hơn khi bạn hiểu rủi ro của vị thế bán và cho rằng giá có thể dao động với xu hướng giảm.
- Neutral phù hợp với thị trường có biến động hai chiều, nhưng vẫn có thể chịu lỗ nếu giá thoát khỏi vùng quá nhanh.

Đòn bẩy càng cao, vùng an toàn càng nhỏ. Một chiến lược có thể trông ổn ở mức đòn bẩy thấp nhưng trở nên rất nhạy với biến động khi tăng đòn bẩy.

## Khi nào bot lưới OKX phù hợp?

Bot lưới có thể phù hợp khi:

- Giá dao động trong một vùng tương đối rõ.
- Tài sản có thanh khoản đủ tốt.
- Bạn không muốn đặt từng lệnh thủ công.
- Bạn có thể theo dõi và điều chỉnh biên độ khi thị trường thay đổi.
- Chi phí giao dịch thấp hơn đáng kể so với khoảng cách giữa các lưới.
- Bạn chấp nhận việc bot có thể giữ tài sản hoặc vị thế mở trong thời gian dài.

Bot lưới kém phù hợp khi:

- Thị trường đang có xu hướng tăng hoặc giảm một chiều rất mạnh.
- Khoảng giá được chọn chỉ dựa trên cảm tính.
- Bạn không hiểu cơ chế thanh lý của futures.
- Bạn dùng toàn bộ vốn cho một bot.
- Bạn kỳ vọng bot tự động bảo vệ tài khoản trong mọi kịch bản.
- Bạn không có kế hoạch xử lý khi giá phá biên trên hoặc biên dưới.

Một số bot có thể dừng khi cặp giao dịch bị hủy niêm yết, trader dẫn đầu dừng bot hoặc hệ thống kích hoạt giới hạn vị thế. Với Futures Grid, thanh lý là một trong những lý do nghiêm trọng nhất khiến bot dừng và vị thế bị đóng cưỡng bức.

## Cách đánh giá bot sau khi chạy

Sau khi tạo bot, đừng chỉ kiểm tra tổng số chu kỳ đã hoàn thành. Hãy theo dõi định kỳ:

1. **Total PnL**: kết quả tổng thể, bao gồm cả phần lãi lỗ chưa thực hiện.
2. **Grid Profit**: lợi nhuận từ các chu kỳ lưới đã hoàn tất.
3. **Unpaired PnL**: phần lãi lỗ chưa gắn với chu kỳ hoàn chỉnh.
4. **Phí giao dịch**: đặc biệt khi bot tạo nhiều lệnh nhỏ.
5. **Funding fee**: nếu sử dụng futures.
6. **Vị thế còn mở**: bot có đang nghiêng quá nhiều về một phía không.
7. **Khoảng cách tới biên giá**: giá hiện tại có sắp thoát vùng không.
8. **Giá thanh lý ước tính**: chỉ áp dụng cho futures.
9. **Thanh khoản của cặp**: thanh khoản giảm có thể làm chất lượng khớp lệnh kém hơn.

Nếu giá phá khỏi vùng giao dịch, việc giữ bot nguyên trạng không phải lúc nào cũng hợp lý. Bạn có thể cần dừng bot, đánh giá lại xu hướng hoặc chờ thị trường quay lại vùng phù hợp. Việc mở rộng biên độ một cách liên tục chỉ để “cứu” một bot đang sai bối cảnh có thể khiến rủi ro lớn hơn.

## Kết luận: nên chọn Spot Grid hay Futures Grid?

Nếu bạn mới tìm hiểu **bot lưới OKX**, Spot Grid là điểm bắt đầu dễ hiểu hơn. Bạn không phải xử lý đòn bẩy, funding fee hay nguy cơ thanh lý. Đổi lại, lợi nhuận và rủi ro chủ yếu đến từ giá trị tài sản mà bot đang nắm giữ.

Futures Grid phù hợp hơn với người đã quen giao dịch hợp đồng và có quy tắc rõ ràng về đòn bẩy, margin, stop-loss và giới hạn vốn. Ba chế độ Long, Short và Neutral cho phép xây dựng nhiều kịch bản hơn, nhưng sự linh hoạt đó đi cùng khả năng thua lỗ nhanh hơn.

Bạn có thể dùng mã mời `CASH20` khi đăng ký qua [👉 Liên kết tham gia OKX với mã CASH20](https://okx.com/join/CASH20). Mức giảm hoặc hoàn phí thực tế cần được kiểm tra trong tài khoản vì còn phụ thuộc điều kiện chương trình và khu vực. Sau đó, hãy bắt đầu với số vốn nhỏ, chọn cặp có thanh khoản tốt, đặt biên độ có lý do và kiểm tra lợi nhuận sau phí thay vì chỉ nhìn vào số lần bot đã giao dịch.
