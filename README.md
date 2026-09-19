# NUTE ANU Platform — Khung loi (Pilot)

Ban demo kien truc de kiem chung tinh kha thi cua khung "core khong doi
- modules mo rong tu do" cho De an chuyen doi ANU cua Truong DHSPKT Nam Dinh.

**Day la ban thi diem voi du lieu gia lap (mock).** Chua ket noi Entra ID,
SharePoint hay he thong nghiep vu that. Muc dich: chung minh dung logic
truoc khi noi voi ha tang that.

## Cau truc

```
core/               <- Loi he thong. KHONG sua khi them module moi.
  identity.py        - kiem tra vai tro / quyen duyet
  roles.yaml          - mock vai tro (production: anh xa Entra ID groups)
  data_gateway.py     - cong duy nhat de module doc du lieu / gui hanh dong
  audit_log.py        - thuc thi "6 cau hoi" bat buoc (muc 4.14g De an)
  registry.py         - quet va nap manifest.yaml cua tat ca module

modules/             <- Moi Smart Box / tac nhan la 1 thu muc rieng
  _template/           - copy thu muc nay khi tao module moi
  training_agent/       - module thi diem: Tac nhan Dao tao

mock_data/           <- Du lieu gia lap thay cho SIS/HRM/Tai chinh that
scripts/
  validate_manifest.py - kiem tra tu dong manifest (Gate G1), dung trong CI
.github/workflows/
  validate.yml         - GitHub Action tu dong chay khi co Pull Request
tests/
  test_pilot_flow.py    - kiem chung toan bo luong: xin quyen -> tu choi/
                           cho phep -> duyet -> ghi log
```

## Chay thu trong VS Code

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Kiem tra tat ca manifest hop le
python scripts/validate_manifest.py

# Chay toan bo test kien truc
python -m pytest tests/ -v
```

Neu ca 5 test deu PASS, nghia la:
1. Module chi doc duoc du lieu trong pham vi khai bao — bi chan neu vuot quyen
2. Hanh dong can duyet se o trang thai "pending" cho den khi dung vai tro duyet
3. Moi hanh dong (kê ca bi tu choi) deu duoc ghi vao audit log du 6 truong
4. He thong tu dong phat hien manifest thieu truong bat buoc

## Them mot Smart Box / tac nhan moi

Xem `modules/_template/README.md`. Tom tat: copy thu muc, dien manifest,
viet code goi qua `core.data_gateway`, tao Pull Request — GitHub Action se
tu kiem tra truoc khi ai do xem xet noi dung.

## Gioi han cua ban thi diem nay

- `roles.yaml` la mock — production phai thay bang Entra ID groups
- `mock_data/*.json` thay the SIS/HRM/Tai chinh that
- Chua co giao dien nguoi dung (UI) — day la lop logic, ghep voi
  `Nute-dashboard` hoac `sbbs-demo` o buoc sau
