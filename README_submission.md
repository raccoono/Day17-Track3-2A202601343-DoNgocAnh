# Submission Notes

Short-term compaction giữ lại constraint `REVIEW-DEADLINE-1600`, bao gồm lịch review vào Friday lúc 16:00, dưới dạng durable note ngay cả khi raw turn cũ đã bị loại khỏi cửa sổ gần nhất. Evidence này được giữ vì nội dung có từ khóa constraint/deadline và marker viết hoa, nên `extract_durable_notes` nhận diện nó là trạng thái cần tồn tại lâu dài. Strategy `buffer` không bền vững cho hội thoại dài vì giữ toàn bộ message khiến số token và context tăng liên tục, trong khi `sliding` giới hạn recent messages nhưng vẫn bảo toàn durable evidence.
