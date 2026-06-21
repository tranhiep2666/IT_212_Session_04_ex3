1. Phân tích vì sao prompt thô dễ bỏ sót lỗi

Prompt chung chung như:

"Mã này bị lỗi gì?"

thường dễ khiến AI bỏ sót lỗi logic trong trường hợp này vì:

Code hoàn toàn hợp lệ về cú pháp
Không có lỗi compile.
Không có cảnh báo từ trình biên dịch.
Không có ngoại lệ (exception) rõ ràng khi chạy.
Lỗi nằm ở logic biên (edge case)

Vòng lặp bên trong khởi tạo:

for (int j = i; j < arr.length; j++)

Khi j == i thì:

arr[i] == arr[j]

luôn đúng vì một phần tử luôn bằng chính nó.

AI có xu hướng đọc code theo mục đích thay vì mô phỏng thực thi
Nhìn tên hàm findDuplicate(), AI dễ suy luận rằng tác giả đang tìm phần tử trùng lặp.
Nếu không được yêu cầu chạy thử bằng dữ liệu cụ thể, AI có thể kết luận sai rằng thuật toán hoạt động bình thường.
Thiếu test case phản chứng

Ví dụ:

{1, 2, 3, 4}
Không có phần tử trùng lặp.

Nhưng chương trình vẫn trả về:

1
Nếu prompt không yêu cầu kiểm tra đầu vào cụ thể, lỗi này rất dễ bị bỏ qua.
Prompt không định hướng phân tích hành vi runtime
AI có thể chỉ kiểm tra cú pháp, cấu trúc vòng lặp và ý định thuật toán.
Không bắt buộc phải truy vết từng bước thực thi (trace execution).
2. Prompt tối ưu mới thiết kế
   Bạn là một Senior Code Auditor chuyên tìm lỗi logic tiềm ẩn trong mã nguồn Java.

Nhiệm vụ:

1. Đọc và phân tích đoạn mã dưới đây.
2. Không chỉ kiểm tra lỗi cú pháp mà phải mô phỏng quá trình thực thi từng bước.
3. Sử dụng test case cụ thể:
   int[] arr = {1, 2, 3, 4};

4. Giải thích chi tiết kết quả thực tế của chương trình với test case trên.
5. Xác định nguyên nhân khiến chương trình trả về kết quả sai.
6. Chỉ ra chính xác dòng mã gây lỗi.
7. Giải thích vì sao đây là lỗi logic (logic bug) chứ không phải lỗi compile.
8. Đề xuất giải pháp tối ưu hơn sử dụng HashSet.
9. Viết lại toàn bộ mã nguồn Java hoàn chỉnh.
10. So sánh độ phức tạp thời gian giữa phiên bản cũ và phiên bản mới.

Mã nguồn:

public class DuplicateFinder {

    public static Integer findDuplicate(int[] arr) {

        for (int i = 0; i < arr.length; i++) {

            for (int j = i; j < arr.length; j++) {

                if (arr[i] == arr[j]) {

                    return arr[i];

                }

            }

        }

        return null;

    }

}

Yêu cầu đầu ra:
- Phân tích từng bước với test case {1,2,3,4}
- Mô tả lỗi logic
- Dòng mã gây lỗi
- Phiên bản Java sửa bằng HashSet
- Phân tích độ phức tạp O(N²) và O(N)
3. Mã nguồn Java đã sửa hoàn chỉnh bằng HashSet
   import java.util.HashSet;
   import java.util.Set;

public class DuplicateFinder {

    /**
     * Tìm phần tử xuất hiện lặp lại đầu tiên khi duyệt từ trái sang phải.
     * Nếu không có phần tử trùng lặp thì trả về null.
     */
    public static Integer findDuplicate(int[] arr) {

        if (arr == null || arr.length == 0) {
            return null;
        }

        Set<Integer> seen = new HashSet<>();

        for (int value : arr) {
            if (seen.contains(value)) {
                return value;
            }
            seen.add(value);
        }

        return null;
    }

    public static void main(String[] args) {

        int[] arr1 = {1, 2, 3, 4};
        int[] arr2 = {1, 2, 3, 2, 5};
        int[] arr3 = {7, 8, 9, 7};

        System.out.println(findDuplicate(arr1)); // null
        System.out.println(findDuplicate(arr2)); // 2
        System.out.println(findDuplicate(arr3)); // 7
    }
}
So sánh độ phức tạp
Phiên bản	Thuật toán	Thời gian	Bộ nhớ
Mã gốc	Hai vòng lặp lồng nhau	O(N²)	O(1)
HashSet	Duyệt một lần + tra cứu HashSet	O(N)	O(N)

Phiên bản HashSet không chỉ khắc phục lỗi logic mà còn cải thiện hiệu năng từ O(N²) xuống O(N) nhờ khả năng kiểm tra phần tử đã xuất hiện trong thời gian trung bình O(1).