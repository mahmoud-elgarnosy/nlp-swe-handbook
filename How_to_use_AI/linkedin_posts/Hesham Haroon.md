الـ AI Agents بتوعنا مش Senior Engineers.

ورقة جديدة من Harvard اسمها SWE-fficiency كشفت إن أحسن الـ agents بتحقق أقل من 0.23x من الـ speedup اللي الـ human experts بيحققوه.

يعني إيه الكلام ده؟

الـ benchmarks الحالية زي SWE-bench بتقيس "هل الـ agent قدر يصلح الـ bug؟" - يعني بتركز على الـ WHAT.

لكن في الواقع، كتير من الكود بيكون شغال صح... بس بطيء. والـ optimization مش عن إصلاح bugs، ده عن الـ HOW.

فالباحثين عملوا benchmark جديد:
- عدد 498 task على 9 repos زي numpy و pandas و scikit-learn
- بدل Pass/Fail، استخدموا Speedup Ratio عشان يقارنوا performance الـ agent بالـ expert
- قضوا 200 ساعة في annotation عشان يعيدوا بناء نفس الـ workloads اللي الـ experts استخدموها

النتايج كانت eye-opening:

→ نسبة 71% من الوقت، الـ models بتختار الـ wrong file أو function للتعديل
→ في pattern اسمه "satisficing" - الـ agent بيلاقي tiny speedup وبيوقف بدل ما يكمل optimization
→ الـ agents بتعمل monkey patches بدل restructuring حقيقي زي ما الـ experts بيعملوا

ليه ده مهم للـ production؟

لو عايز agent يشتغل asynchronously في الـ background يحسّن الـ codebase، لازم يفهم الـ tradeoffs ويفكر زي Senior Engineer مش بس يعدي tests.