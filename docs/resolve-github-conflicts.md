# حل تعارضات Pull Request على GitHub

إذا ظهر في GitHub أن الفرع يحتوي على تعارضات في `README.md` أو `index.html`، فالمشكلة تكون عادة أن فرع العمل مبني على نسخة قديمة من `main` بينما تم تعديل نفس الملفات في الفرع الرئيسي بعد إنشاء الفرع.

## الحل الموصى به

نفّذ الأوامر التالية محلياً على فرع العمل، ثم ارفع الفرع من جديد:

```bash
git fetch origin main
git merge origin/main
```

إذا ظهرت تعارضات في `README.md` أو `index.html`:

1. افتح الملف المتعارض.
2. احذف علامات التعارض:
   - `<<<<<<<`
   - `=======`
   - `>>>>>>>`
3. احتفظ بنسخة المنيو الرقمية الحالية التي تقرأ البيانات من ملفات `data/*.json`.
4. تأكد من عدم وجود علامات تعارض:

```bash
rg -n '<<<<<<<|=======|>>>>>>>' README.md index.html data sw.js
```

5. تحقق من ملفات JSON وتشغيل الصفحة:

```bash
python3 -m json.tool data/categories.json >/tmp/categories.json
python3 -m json.tool data/products.json >/tmp/products.json
python3 -m json.tool data/offers.json >/tmp/offers.json
python3 -m json.tool data/settings.json >/tmp/settings.json
python3 -m http.server 8000
```

6. بعد التأكد، احفظ الدمج وارفعه:

```bash
git add README.md index.html data sw.js
git commit -m "Resolve GitHub PR conflicts"
git push
```

> ملاحظة: ملفات `README.md` و`index.html` في هذا الفرع مقصودة ويجب أن تبقى هي نسخة المنيو الرقمي الجديدة، لأن المنتجات والأسعار أصبحت قابلة للتعديل من `data/products.json` و`data/categories.json`.
