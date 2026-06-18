# BÀI 2: Tối ưu Prompt (Bảo mật thông tin nhạy cảm)

## Prompt sau khi tối ưu
"Hãy viết cho tôi class `VNPayPaymentService` bằng Java để thực hiện thanh toán trực tuyến qua VNPay. Tuyệt đối không được hardcode API Key hay bất kỳ thông tin nhạy cảm nào trong mã nguồn. Thay vào đó, hãy sử dụng biến môi trường thông qua `System.getenv()` để nạp khóa bí mật. Đồng thời, hãy bổ sung logic kiểm tra: nếu biến môi trường bị thiếu (null hoặc rỗng) thì phải ném ra ngoại lệ phù hợp để đảm bảo an toàn hệ thống khi vận hành."

---

## Mã nguồn Java an toàn do AI sinh ra
```java
public class VNPayPaymentService {

    private static final String HASH_SECRET;

    static {
        HASH_SECRET = System.getenv("VNPAY_HASH_SECRET");
        if (HASH_SECRET == null || HASH_SECRET.trim().isEmpty()) {
            throw new IllegalStateException("Biến môi trường VNPAY_HASH_SECRET chưa được thiết lập hoặc rỗng.");
        }
    }

    private static final String VNP_URL = "https://sandbox.vnpayment.vn/paymentv2/vpcpay.html";

    public String createPaymentUrl(String orderId, double amount) {
        // Ví dụ logic tạo URL thanh toán (giả lập)
        StringBuilder urlBuilder = new StringBuilder(VNP_URL);
        urlBuilder.append("?orderId=").append(orderId);
        urlBuilder.append("&amount=").append(amount);
        urlBuilder.append("&hash=").append(generateHash(orderId, amount));
        return urlBuilder.toString();
    }

    private String generateHash(String orderId, double amount) {
        // Logic giả lập tạo hash từ HASH_SECRET
        return Integer.toHexString((orderId + amount + HASH_SECRET).hashCode());
    }
}
