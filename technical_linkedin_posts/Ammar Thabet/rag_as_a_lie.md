"من RAG لـ RAG-as-a-Lie: إزاي Benchmarks حلوة بتغطي على System فاشل في الواقع" 

مش بمجرد إن السيستم فيه RAG و RAGAS score رقم حلو يبقى احنا كده عملنا أحسن حاجة في الدنيا؟ 
الحقيقة إن في فرق كبير بين RAG معمول عشان يطلع أرقام تجيب لايكات على LinkedIn، وRAG تقدر ترميه في production وسط users حقيقيين وتنام وأنت مطمن. أغلب الـ RAG systems اللي بشوفها متضبطة على test set نضيف، أسئلة متوقعة، وground truth معمولة بعناية زيادة عن اللزوم، فتاخد 0.8 و0.9 وتبان إنها “state-of-the-art”. 

بس أول ما المستخدم الحقيقي يدخل بسؤال غامض شوية، لهجة، typo، أو wording مختلف، الretriever يجيب context مش relevant والـ LLM يكمل الهلوسة بثقة. 

المشكلة إننا ساعات بنقيّم السيستم على نفس الداتا اللي بنينا منها الـ index، ومانحطش domain drift ولا knowledge updates ولا حتة الـ long-tail queries في الصورة أصلاً. 

كمان بنقيس retrieval و generation كأنهم وحدات منفصلة، بس مانبصش على السؤال الأهم: هل المستخدم فعلاً أخد قرار صح بعد الإجابة؟ ولا طلع من الchat أضيع من الأول؟ 

المفروض هنا نبص على ٣ حاجات:
أولاً: الretrieval نفسه، هل بيغطي الـ edge cases؟ هل بيتعامل مع synonyms، لهجات، وnoise ولا لأ؟ 

ثانياً: الgeneration، هل الـ LLM فعلاً بيستخدم السياق ولا بيرجع للpriors ويخترع؟ وهل في guardrails وdefense-in-depth ضد الهلوسة؟ 

ثالثاً: الproduct، هل في task success حقيقي، monitoring، feedback loop، وincident reviews ولا كله واقف عند “الmetric طلعت كويسة خلاص شكرًا”؟ لو الـ RAG عندك واخد درجات عالية على Benchmarks بس بيقع أول ما يواجه user حقيقي، فأنت غالباً بنيت RAG-as-a-Lie مش RAG System. 

متفتخرس انك جايب أعلى score، المهم انك تصطاد أسوأ failure scenario قبل ما العميل يشوفه في الـ production.