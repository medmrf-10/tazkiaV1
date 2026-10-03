# tazkiaV1 — خدمة استعلام الرقائق والتزكية

78 كتاباً من كتب الرقائق والتزكية مقطعة عند حدود فهارسها الأصلية — استعلام بثلاثة طلبات، بلا تحميل ولا مفاتيح ولا حد:

```
١) كل الكتب المتوفرة:        GET  /api/index.json
٢) فهرس الكتاب المختار:      GET  /api/books/<id>/toc.json
٣) محتوى العنوان المختار:    GET  /api/books/<id>/parts/<NNNN>.json.gz
```

مثال:
```
curl -s https://medmrf-10.github.io/tazkiaV1/api/index.json
# ← {_api, n_books, books:[{id,slug,title,author,death,nparts,m,t,a,z}…]}
curl -s https://medmrf-10.github.io/tazkiaV1/api/books/<id>/toc.json
# ← {id,title,author,toc:[{i,t,d,pg,j,sz}…]}  i=الجزء t=العنوان pg=صفحة الطبعة j=الجزء sz=حجم~بايت
curl -s https://medmrf-10.github.io/tazkiaV1/api/books/<id>/parts/0005.json.gz | gunzip
# ← {i,t,pg,j,b,text,fn?}
```

**حقول الجزء**: `b` = دقة الحدود: `m` علامة فهرس داخلية (دقيق)، `t` حُدّد بمطابقة نص العنوان في صفحته (دقيق)، `a` العنوان لم يُوجد — النص هو نافذة الصفحة المُدّعاة (كامل المحتوى، قد يسبقه سطر من الجزء السابق)، `z` عنوان صفري ملتصق بالتالي. `fn` = حواشي الصفحات `{page_num: نص}` عند وجودها. `index.json._api` = نفس هذا التوصيف آلياً.

**بحث نصي**: `GET /api/idx/<أول-حرفين-مطبّعين>.json.gz` ← `{كلمة:[[كتاب,جزء]…]}` — تقاطع القوائم عندك. التطبيع: بلا تشكيل ولا تطويل، أإآٱ→ا، ى→ي، ة→ه؛ السوابق لا تُقلع (مرّر الصيغ بنفسك). السياسة كاملة في `api/idx/_meta.json`.

المصدر: shamelaPure (shamela.ws) — التقسيم عند فهارس المؤلفين أنفسهم.
