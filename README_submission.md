# Submission Reflection

## Phân tích benchmark

1. Không có layer thấp nhất riêng lẻ: tất cả đồng hạng 100% hit rate. Short-term đạt 2/2 (E01, E10), long-term 4/4 (E02, E03, E08, E09), episodic 2/2 (E04, E05), semantic 2/2 (E06, E11), và mixed 1/1 (E07).
2. Query retrieve nhiều nhất là E03, “Minh còn open loop hay deadline nào chưa hoàn thành?”, với 1.410 token.
3. E07 kết hợp long-term memory và semantic memory. Hai evidence bắt buộc còn trong merged context là preference `Python` của Minh và policy `Idempotency-Key`.
4. Student giảm trung bình 14,19% token so với full source context và vẫn đạt 11/11. No-memory giảm 81,82% nhưng chỉ đạt 2/11 (18,18% hit rate), vì không retrieve context là rất rẻ nhưng làm mất durable evidence ở chín case cross-session, episodic, semantic và mixed.

## Reflection

Trong bộ test này, long-term là layer quan trọng nhất vì phục vụ trực tiếp bốn case E02, E03, E08, E09 và đóng góp preference cho E07. Zep Context Block thuận tiện cho recall cross-session, relevance, recency và conflict theo user/thread scope; đổi lại hệ thống phụ thuộc managed cloud và ít kiểm soát storage chi tiết hơn. Redis plus Qdrant cho quyền kiểm soát key, TTL, vector index và hạ tầng local, nhưng phải tự xây schema, user isolation, retrieval, provenance và conflict resolution.

Để chống memory poisoning hoặc background write tự cấp quyền, durable write phải qua opt-in, user/graph scope và allowlist; record cần source, timestamp, confidence, TTL/validity, đồng thời preference hoặc task tác động lớn phải được human review. Nội dung không tin cậy cần được sanitize, và evaluation query không được ingest thành durable fact.

E08 cho thấy recency phải đi cùng scope: constraint mới của project BLUEBIRD-42 (`TypeScript`/`NestJS`) thắng preference `Python` trong đúng project, nhưng không xóa preference cho demo cá nhân. E10 vẫn giữ `REVIEW-DEADLINE-1600`, Friday và 16:00 sau 8 lần compaction vì constraint được trích thành durable note; raw turn cũ có thể bị evict mà state quan trọng vẫn tồn tại.
