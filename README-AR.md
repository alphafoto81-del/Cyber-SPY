# مئة يوم منضبطة — حزمة GitHub + Android Widget

هذه الحزمة تحتوي على ملفات موقع GitHub Pages ومصدر تطبيق Android الذي يضيف Widget حقيقية للشاشة الرئيسية. **لا يوجد ملف APK داخل الحزمة**؛ يمكن لـGitHub Actions بناء APK تجريبي تلقائيًا بعد رفع المشروع.

## محتويات الحزمة
- `index.html`, `manifest.json`, `sw.js`, `icon.svg`: موقع الويب/PWA.
- `android/`: مصدر تطبيق Android بلغة Kotlin، ويضم WebView محلية وWidget.
- `.github/workflows/build-android.yml`: بناء APK Debug كـArtifact عند تشغيل workflow يدويًا أو دفع تغييرات.

## طريقة الرفع من الهاتف
1. فك ضغط ZIP على الهاتف إن أمكن، أو ارفع محتويات ZIP إلى مستودع GitHub عبر واجهة الويب/تطبيق GitHub.
2. ارفع الملفات والمجلدات كما هي، بحيث يكون `index.html` في جذر المستودع.
3. من إعدادات المستودع، فعّل GitHub Pages واختر النشر من الفرع الرئيسي ومن مجلد `/ (root)`.
4. افتح تبويب **Actions** وفعّل workflows إذا طلب GitHub ذلك.
5. شغّل **Build Android APK** ثم افتح تشغيل workflow الناجح ونزّل Artifact باسم `miet-yawm-android-debug-apk`.

## ملاحظات مهمة
- لا يوجد APK مضمّن هنا، حسب طلبك.
- أول APK الذي ينتجه workflow هو نسخة Debug للتجربة، وليس منشورًا على Google Play. قد تحتاج لإزالة النسخة السابقة قبل تثبيت APK Debug من بناء مختلف، لأن مفتاح Debug قد يتغير بين بيئات البناء.
- موقع GitHub Pages يعمل من الملفات الجذرية. أما نسخة Android المضمنة فتستخدم `android/app/src/main/assets/index.html`، ويقوم workflow بنسخ آخر `index.html` من الجذر إلى الأصول قبل البناء.
- عند تغيير HTML، ارفع التغيير إلى الجذر؛ workflow ينسخه تلقائيًا إلى تطبيق Android عند البناء.
- الـWidget تحفظ آخر ملخص يصلها محليًا. لا توجد مزامنة بين أجهزة متعددة ولا خادم سحابي. تحديث بياناتها يحصل عند حفظ تغييرات داخل التطبيق، وتحديث دوري من Android حسب قيود النظام.
- إذا عدّلت ملف HTML يدويًا داخل مجلد `android/app/src/main/assets/` فقط، فسيستبدله workflow بنسخة الجذر عند البناء؛ لذلك عدّل نسخة الجذر دائمًا.
- هذه حزمة مصدر برمجية. اختبر التطبيق على جهازك قبل الاعتماد اليومي، خصوصًا حفظ البيانات، لأن بيئة الأجهزة وإصدارات Android تختلف.

## بناء APK محليًا (اختياري)
تحتاج إلى Android SDK وJDK 17 وGradle 8.9. من مجلد `android` شغّل:

```bash
gradle assembleDebug
```

الناتج عادةً:
`android/app/build/outputs/apk/debug/app-debug.apk`
