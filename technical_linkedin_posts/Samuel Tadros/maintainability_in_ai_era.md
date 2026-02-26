1 month ago • Visible to anyone on or off LinkedIn

أوقات بنخدع نفسنا لما نستخدم AI: "أنا فاهم الكود ده كويس مش محتاج documentation".

الحقيقة إن "الكود اللي محدش تعب فيه بيتنسي بسرعة". لما الـ AI بيكتب هو بيوفر عليك وقت الكتابة بس بيسرق منك الـ "Mental Model" وفهمك العميق للكود بيعمل إيه وليه.

🛑 الكود اللي طالع من AI غالباً بيكون شغال بس بعد شهرين لما ترجع له هتحس إنه "غريب" عنك. هتقف قدام دالة معقدة وتسأل: "هو ليه استخدم الطريقة دي بالذات؟".
وهنا بتبدأ رحلة الـ Legacy Code بدري جداً.

عشان تضمن إن الكود ده يعيش ويكون Maintainable طبق قواعد الDocumentation دي:

👉 1. Document the "Why", not the "How"

الـ AI شاطر إنه يشرح الكود بيعمل إيه (Step by step). دي وظيفة الكود نفسه لو مكتوب نظيف.
دورك أنت تكتب الـ Intent والـ Decisions.
بدل ما تكتب: "This function loops through users", اكتب: "Using a batch loop here to prevent hitting API rate limits during sync".
ده اللي الـ AI ميعرفش يقوله لأنه ميعرفش الـ Context الخارجي.

👉 2. The Prompt is the Documentation

لو خليت الـ AI يكتب Logic معقد (زي Regex أو Mathematical Formula)، انسخ الـ Prompt اللي استخدمته وحطه في الـ Comments.
الـ Prompt هو "الوصفة" اللي طلعت الكود. وجوده بيساعد أي حد يجي بعدك يفهم إنت طلبت إيه، وممكن كمان يستخدمه عشان يعدل الكود أو يعمل له Refactoring بنفس الـ Constraints.

👉 3. Sanitize the AI Comments (نضف وراه)

لما تطلب من الـ AI يكتب Comments غالباً بيكتب "شعر" وكلام حشو ملوش لازمة (Noise).
بلاش تسيب كومنتات زي: "In this snippet, we are going to initialize the variable...".
امسح الزيادات دي فوراً. الكومنت لازم يكون قيمة مضافة مش مجرد تكرار للكود بالانجليزي.

𝐂𝐨𝐝𝐞 𝐢𝐬 𝐫𝐞𝐚𝐝 𝟏𝟎𝐱 𝐦𝐨𝐫𝐞 𝐭𝐡𝐚𝐧 𝐢𝐭 𝐢𝐬 𝐰𝐫𝐢𝐭𝐭𝐞𝐧. 𝐀𝐈 𝐦𝐚𝐤𝐞𝐬 𝐰𝐫𝐢𝐭𝐢𝐧𝐠 𝐜𝐡𝐞𝐚𝐩 𝐬𝐨 𝐦𝐚𝐤𝐞 𝐫𝐞𝐚𝐝𝐢𝐧𝐠 𝐢𝐭 𝐲𝐨𝐮𝐫 𝐩𝐫𝐢𝐨𝐫𝐢𝐭𝐲.

💡 الخلاصة

الDocumentation مش رفاهية ده العقد اللي بينك وبين الـ Future Self بتاعك وزمايلك كمان
الـ AI ممكن يكتب الكود بس أنت اللي لازم تكتب "الغرض" اللي ورا الكود.