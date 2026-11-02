# Đơn vị hành chính Việt Nam sau sắp xếp 2025 (34 tỉnh, 3.321 xã, phường)

*[English below](#english)*

Bộ dữ liệu mở về đơn vị hành chính Việt Nam từ 01/07/2025: 34 tỉnh, thành phố; 3.321 xã, phường, đặc khu; bảng đối chiếu xã cũ sang xã mới; mã bưu chính mới. Có cập nhật các thay đổi năm 2026 (Đồng Nai, Quảng Ninh, Bắc Ninh lên thành phố trực thuộc Trung ương, xã lên phường).

- **Dữ liệu theo văn bản có hiệu lực đến:** 20/09/2026
- **Ngày dựng:** 02/11/2026
- **Giấy phép:** [CC BY 4.0](LICENSE), xem [ghi nguồn](#ghi-nguồn) và [NOTICE](NOTICE)
- **Tra cứu trực tuyến, đổi địa chỉ cũ sang mới:** <https://diachiviet.com/doi-dia-chi-moi/>
- **Trang dữ liệu:** <https://diachiviet.com/du-lieu-mo/>
- **Bản trên Hugging Face:** <https://huggingface.co/datasets/chuongkt/vietnam-administrative-units>

## Các file

| File | Số dòng | Nội dung |
|---|---:|---|
| `data/tinh.csv`, `.json` | 34 | Tỉnh, thành phố: mã, tên, loại, số đơn vị cấp xã |
| `data/xa.csv`, `.json` | 3.321 | Xã, phường, đặc khu: mã, tên, loại, tỉnh, mã bưu chính, diện tích, quy mô dân số, nghị quyết |
| `data/xa-cu-moi.csv`, `.json` | 10.618 | Đơn vị cấp xã trước 01/07/2025 và đơn vị mới tương ứng, ghi rõ "một phần", "phần còn lại" |
| `data/provinces/xa-{tinh}.csv` | 34 file | Cùng cột với `xa.csv`, mỗi file một tỉnh, thành |
| `data/meta.json` | | Ngày dựng, số dòng, danh sách cột |

CSV mã hoá UTF-8 có BOM (Excel mở đúng tiếng Việt), phân cách bằng dấu phẩy.

## Các cột

**`tinh`:** `ma_tinh` (2 chữ số, chuỗi), `ten`, `ten_day_du`, `loai`, `so_don_vi_cap_xa`, `url`

**`xa`:** `ma_xa` (5 chữ số, chuỗi, ví dụ "00004"), `ten`, `loai`, `ma_tinh`, `ten_tinh`, `ma_buu_chinh`, `dien_tich_km2`, `quy_mo_dan_so`, `nghi_quyet`, `url`

**`xa-cu-moi`:** `ma_xa_cu`, `ten_xa_cu`, `quan_huyen_cu`, `tinh_cu`, `ma_xa_moi`, `ten_xa_moi`, `ma_tinh`, `ten_tinh`, `pham_vi`, `ten_danh_muc_cu`

- `ma_xa_cu`, `quan_huyen_cu` để trống khi chưa xác định chắc chắn.
- `pham_vi`: "toàn bộ", hoặc ghi chú như "một phần", "phần còn lại". Một đơn vị cũ bị chia cho nhiều đơn vị mới thì có nhiều dòng.
- `ten_danh_muc_cu`: tên trong danh mục bản 01/03/2025 khi khác `ten_xa_cu` (cách viết khác), dùng để đối chiếu, tìm kiếm.
- `quy_mo_dan_so` là số liệu của đề án sắp xếp, không phải số điều tra dân số.

**Nên nối bảng bằng mã** (`ma_xa`, `ma_tinh`), không nối bằng tên, vì có nhiều xã trùng tên. Đọc cột mã ở dạng chuỗi để giữ số 0 ở đầu:

```python
import pandas as pd

xa = pd.read_csv("data/xa.csv", dtype={"ma_xa": str, "ma_tinh": str, "ma_buu_chinh": str})
cu_moi = pd.read_csv("data/xa-cu-moi.csv", dtype=str)

# Phường Bến Nghé (Quận 1 cũ) nay thuộc đơn vị nào?
print(cu_moi[cu_moi["ten_xa_cu"] == "Phường Bến Nghé"][["ten_xa_cu", "quan_huyen_cu", "ten_xa_moi", "pham_vi"]])
```

## Nguồn

- Danh mục và mã số đơn vị hành chính: Quyết định 19/2025/QĐ-TTg và các văn bản cập nhật của Cục Thống kê; các nghị quyết sắp xếp đơn vị hành chính của Quốc hội, Ủy ban Thường vụ Quốc hội.
- Mã bưu chính: Quyết định 2334/QĐ-BKHCN ngày 24/08/2025 của Bộ Khoa học và Công nghệ.
- Diện tích, quy mô dân số cấp xã: số liệu trong các nghị quyết sắp xếp cấp xã năm 2025 (tổng hợp qua sapnhap.bando.com.vn).
- Danh mục trước 01/07/2025 (bản 01/03/2025): [thanglequoc/vietnamese-provinces-database](https://github.com/thanglequoc/vietnamese-provinces-database), giấy phép MIT, xem [NOTICE](NOTICE).

## Giới hạn

Đây là dữ liệu tham khảo, có thể có sai sót. Giấy tờ chính thức theo xác nhận của cơ quan có thẩm quyền.

- Một đơn vị cũ bị chia thì bảng không cho biết từng số nhà thuộc phần nào.
- Tên đơn vị cũ ghi theo nghị quyết; khoảng 200 tên khác cách viết với danh mục cũ và chưa được đối chiếu từng dòng với toàn văn nghị quyết.
- Một số đơn vị cũ chưa xác định được mã, huyện cũ nên để trống.

Báo lỗi: mở [Issue](../../issues) hoặc qua <https://diachiviet.com/lien-he/>.

## Ghi nguồn

Dùng tự do, kể cả mục đích thương mại, với điều kiện ghi nguồn. Mẫu:

> Nguồn: Địa Chỉ Việt (diachiviet.com), dữ liệu mở đơn vị hành chính, CC BY 4.0 - https://diachiviet.com/du-lieu-mo/

---

## English

Open dataset of Vietnam's administrative units after the 2025 reorganisation, in effect from 1 July 2025: 34 provinces and centrally governed cities, 3,321 communes, wards and special zones, an old-to-new commune mapping, and new postal codes. Includes 2026 changes (Dong Nai, Quang Ninh and Bac Ninh becoming centrally governed cities; communes becoming wards).

- **Data reflects legal documents in effect up to:** 20 September 2026
- **License:** [CC BY 4.0](LICENSE), see [NOTICE](NOTICE)
- **Online lookup and old-to-new address converter (Vietnamese):** <https://diachiviet.com/doi-dia-chi-moi/>

| File | Rows | Content |
|---|---:|---|
| `data/tinh.csv`, `.json` | 34 | Provinces: code, name, type, number of commune-level units |
| `data/xa.csv`, `.json` | 3,321 | Communes, wards, special zones: code, name, type, province, postal code, area, population, resolution |
| `data/xa-cu-moi.csv`, `.json` | 10,618 | Pre-1 July 2025 communes mapped to new units, with partial-area notes (`pham_vi`) |
| `data/provinces/xa-{tinh}.csv` | 34 files | Same columns as `xa.csv`, one file per province |

Column names are Vietnamese: `ma` = code, `ten` = name, `loai` = type, `tinh` = province, `xa` = commune, `cu` = old, `moi` = new, `ma_buu_chinh` = postal code, `dien_tich_km2` = area, `quy_mo_dan_so` = population (from the reorganisation proposals, not census data), `nghi_quyet` = resolution, `pham_vi` = scope ("toàn bộ" = whole, "một phần" = part, "phần còn lại" = remainder).

CSV files are UTF-8 with BOM. Join tables by code, not by name, because many communes share names, and read code columns as strings to keep leading zeros.

This is reference data and may contain errors; official documents prevail. Report errors via [Issues](../../issues).

**Attribution:** Source: Địa Chỉ Việt (diachiviet.com), Vietnam administrative units open data, CC BY 4.0 - https://diachiviet.com/du-lieu-mo/
