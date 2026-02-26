# CoreTrust iOS History & The TrollStore Era

A comprehensive technical report detailing the history of CoreTrust vulnerabilities in iOS, the bypass of Apple Mobile File Integrity (AMFI), and the mechanics behind TrollStore's permanent sideloading capabilities.

تقرير تقني مفصل يوثق تاريخ ثغرات CoreTrust في نظام iOS، وكيفية تخطي حماية AMFI، والآلية التقنية التي مكّنت مشروع TrollStore من تحقيق التثبيت الدائم للتطبيقات.

## Read the Full Report / قراءة التقرير الكامل

Select your preferred language to read the detailed technical breakdown:
اختر لغتك المفضلة لقراءة التقرير التقني المفصل:

* **[Read the report in English (EN)](./CoreTrust-History-EN.md)**
* **[قراءة التقرير باللغة العربية (AR)](./CoreTrust-History-AR.md)**

## Overview / نظرة عامة

This repository serves as an educational and historical archive documenting one of the most significant paradigm shifts in iOS security research. It explores how a logic flaw in the signature verification process allowed the community to achieve "perma-signing" without requiring a traditional root-level jailbreak.

يُعد هذا المستودع أرشيفاً تعليمياً وتاريخياً يوثق واحدة من أهم النقلات النوعية في مجال البحث الأمني لنظام iOS. يستعرض التقرير كيف سمح خلل منطقي في عملية التحقق من التواقيع للمجتمع بتحقيق "التثبيت الدائم" دون الحاجة لكسر حماية النظام (الجيلبريك) بشكله التقليدي.

## Key Topics Covered / أبرز المواضيع المطروحة

* **CoreTrust & AMFI:** Understanding the iOS chain of trust. / فهم سلسلة الثقة في نظام iOS.
* **TrollStore Mechanics:** How it works under the hood. / كيف يعمل المشروع تقنياً.
* **Vulnerability Analysis:** CVE-2022-26766 and CVE-2023-41991. / تحليل الثغرات المستخدمة.
* **Installation Vectors:** From TrollInstaller to TrollRestore. / مسارات التثبيت المختلفة.
* **The Future of iOS Security:** The impact of SPTM and TXM in iOS 17+. / مستقبل الحماية في وجود أنظمة SPTM و TXM.

## Target Audience / الفئة المستهدفة

This report is written for security researchers, iOS developers, and tech enthusiasts interested in reverse engineering, Apple's security architecture, and the history of iOS exploitation.

كُتب هذا التقرير للباحثين الأمنيين، مطوري أنظمة iOS، والمهتمين بالهندسة العكسية والبنية الأمنية لأنظمة Apple وتاريخ ثغراتها.

## Disclaimer / إخلاء مسؤولية

The information provided in this repository is for educational and historical documentation purposes only. 

المعلومات الواردة في هذا المستودع هي لأغراض تعليمية وللتوثيق التاريخي التقني فقط.
