# Preface: Why I Wrote This Book (서문: 왜 이 책을 썼는가)

## The Pattern I Kept Seeing (반복되는 패턴)
For over 25 years, I’ve been building software.
I started as a junior developer at the turn of the millennium, worked my way up to lead research institutes, and have guided countless projects across manufacturing, AI, smart factories, and security systems. I’ve worked with teams in Korea, Taiwan, Japan, and the United States. I’ve seen what works and what doesn’t.
And through all of it, I kept seeing the same pattern.
**Good developers making bad decisions under pressure.**

25 년 넘게 소프트웨어를 만들어왔다. 
2000년에 주니어 개발자로 시작해서, 연구소장을 거쳐, 제조업, AI, 스마트팩토리, 보안 시스템까지 수많은 프로젝트를 이끌었다. 한국, 대만, 일본, 미국에서 팀들과 일했다. 무엇이 되고 무엇이 안 되는지 봐왔다. 
그리고 그 모든 경험을 통해, 같은 패턴을 계속 목격했다. 
**좋은 개발자가 압박 속에서 나쁜 결정을 내린다.**

Not because they lacked skill. Not because they didn’t know better. But because when the deadline loomed, when the requirements changed at the last minute, when the manager asked “when will it be done?”—something happened. Their careful thinking gave way to rushed reactions. Their systematic approach collapsed into quick fixes.
I watched talented engineers reach for global variables when they knew better. I saw welldesigned systems become tangled messes in the final weeks before delivery. I witnessed the same mistakes repeated across different companies, different technologies, different decades.
And I realized: this wasn’t a skill problem. It was a thinking problem.

실력이 없어서가 아니다. 몰라서가 아니다. 하지만 마감이 다가오면, 요구사항이 막판에 바뀌면, 매니저가 “언제 끝나요?”라고 물으면—무언가가 일어난다. 신중한 사고가 급한 반응으로 바뀐다. 체계적인 접근이 임시방편으로 무너진다. 
재능 있는 엔지니어가 더 나은 방법을 알면서도 전역 변수를 쓰는 것을 봤다. 잘 설계된 시스템이 출시 직전 몇 주 만에 스파게티 코드가 되는 것을 봤다. 다른 회사, 다른 기술, 다른 시대에서 같은 실수가 반복되는 것을 목격했다. 
그리고 깨달았다: 이것은 스킬 문제가 아니다. 사고방식 문제다. 

## The Question That Haunted Me (나를 괴롭힌 질문)
In 1996, after returning from military service, I took a course on software engineering. Unlike database or networking courses that focused on specific technologies, software engineering asked deeper questions: 
What makes software good? How do we build systems that last?
These questions became my obsession. “What does it mean to build good software?” has been my lifelong inquiry. Not just software that works—any programmer can make software that works. But software that remains good when requirements change. Software that welcomes new features instead of fighting them. Software that survives contact with reality.

1996년, 군 복무 후 소프트웨어 공학 수업을 들었다. 데이터베이스나 네트워킹처럼 특정 기술에 집중하는 과목과 달리, 소프트웨어 공학은 더 깊은 질문을 던졌다: 
무엇이 좋은 소프트웨어인가? 어떻게 지속되는 시스템을 만드는가? 
이 질문이 내 집착이 되었다.  “좋은 소프트웨어를 만든다는 것은 무엇인가?”가 평생의 탐구가 되었다. 단지 작동하는 소프트웨어가 아니다—아무 프로그래머나 작동하는 소프트웨어는 만들 수 있다. 하지만 요구사항이 바뀌어도 좋은 소프트웨어. 새 기능을 싸우지 않고 환영하는 소프트웨어. 현실과 만나도 살아남는 소프트웨어. 

I completed my master’s degree in software engineering. I led research institutes for over 15 years. I conducted software engineering studies with my teams for over a decade. I kept asking the question.
And slowly, through countless projects and failures and successes, I began to find answers.

소프트웨어 공학 석사를 마쳤다. 15 년 넘게 연구소를 이끌었다. 10 년 넘게 팀과 소프트웨어 공학 스터디를 진행했다. 계속 질문했다. 
그리고 천천히, 수많은 프로젝트와 실패와 성공을 통해, 답을 찾기 시작했다. 

## The Three Discoveries (세 가지 발견)
The first breakthrough came when I encountered Daniel Kahneman’s work on how humans think.
Kahneman, a Nobel laureate, revealed that we have two modes of thinking: System 1 (fast, automatic, error-prone) and System 2 (slow, deliberate, accurate). Under pressure, System 1 dominates. This explained everything I had observed. When deadlines pressed, developers weren’t making conscious decisions to use global variables—their fast-thinking System 1 was grabbing the first solution that came to mind. 

첫 번째 돌파구는 Daniel Kahneman 의 인간 사고에 대한 연구를 만났을 때였다. 
노벨상 수상자 Kahneman 은 우리에게 두 가지 사고 모드가 있다고 밝혔다: System 1(빠르고, 자동적이고, 오류가 많은)과 System 2(느리고, 의도적이고, 정확한). 압박 속에서는 System 1 이 지배한다. 이것이 내가 관찰한 모든 것을 설명했다. 마감이 압박할 때, 개발자들은 전역 변수를 쓰겠다는 의식적 결정을 내리는 게 아니었다—그들의 빠른 사고 System 1 이 떠오르는 첫 번째 해결책을 잡는 것이었다. 

The second breakthrough came from Donella Meadows and systems thinking. Meadows taught me to see the whole, not just the parts. She showed me that the most powerful intervention point in any system is at the design level—before implementation begins. A complete service chain designed upfront could prevent chaos during implementation. “More is better than missing” became a core principle.

두 번째 돌파구는 Donella Meadows 와 시스템 사고에서 왔다. 
Meadows 는 부분이 아닌 전체를 보는 법을 가르쳐주었다. 어떤 시스템에서든 가장 강력한 개입 지점은 설계 단계—구현이 시작되기 전—라는 것을 보여주었다. 미리 완성된 서비스 체인이 구현 중의 혼란을 방지할 수 있다. “부족한 것보다 많은 게 낫다”가 핵심 원칙이 
되었다. 

The third breakthrough came from Genrich Altshuller and TRIZ. Altshuller, who analyzed millions of patents, discovered that most problems involve contradictions that people try to compromise around. “Fast development” versus “high quality” seems like a tradeoff. But TRIZ taught me: don’t compromise—resolve the contradiction. By separating design time (slow and careful) from implementation time (fast but guided), we could achieve both speed and quality.

세 번째 돌파구는 Genrich Altshuller 와 TRIZ 에서 왔다. 
수백만 개의 특허를 분석한 Altshuller 는 대부분의 문제가 사람들이 타협하려는 모순을 포함한다는 것을 발견했다. “빠른 개발” vs “높은 품질”은 트레이드오프처럼 보인다. 하지만 TRIZ 가 가르쳐주었다: 타협하지 말고—모순을 해결하라. 설계 시간(느리고 신중하게)과 구현 시간(빠르지만 가이드된)을 분리함으로써, 속도와 품질 둘 다 달성할 수 있다. 


## From Theory to Practice (이론에서 실천으로)
Understanding these theories was one thing. Applying them was another.
Over the years, working across industries—SCM systems, AI platforms, smart factories, AR/VR solutions, security applications—I developed three practical methodologies: 

이 이론들을 이해하는 것은 한 가지였다. 적용하는 것은 또 다른 것이었다. 
수년에 걸쳐, 여러 산업에서 일하면서—SCM 시스템, AI 플랫폼, 스마트팩토리, AR/VR 솔루션, 보안 애플리케이션—세 가지 실용적인 방법론을 개발했다: 

- ROD (Responsibility-Oriented Design): Design complete service chains before implementation, eliminating the “missing pieces” that trigger System 1 panic responses.
ROD (Responsibility-Oriented Design): 구현 전에 완전한 서비스 체인을 설계하여, System 1 패닉 반응을 촉발하는 “누락된 조각”을 제거한다. 
- TFD (Test-First Development): Treat requirements as tests, creating feedback loops that catch errors early and guide implementation.
TFD (Test-First Development): 요구사항을 테스트로 취급하여, 오류를 일찍 잡고 구현을 가이드하는 피드백 루프를 만든다. 
- DGTF (Don’t Go Too Fast): Maintain System 2 thinking even under pressure, recognizing triggers and pausing before reacting.
DGTF (Don’t Go Too Fast): 압박 속에서도 System 2 사고를 유지하고, 트리거를 인식하고 반응하기 전에 멈춘다. 

These methodologies worked. Teams that adopted them produced better software with fewer bugs. Projects became predictable. The frantic last-minute scrambles disappeared. Engineers went home on time.
I taught these methods to my teams. I conducted studies. I refined the approaches based on real-world feedback. But I kept them internal, shared only with colleagues. 

이 방법론들은 효과가 있었다. 이것을 채택한 팀들은 버그가 적은 더 좋은 소프트웨어를 만들었다. 프로젝트가 예측 가능해졌다. 막판의 미친 듯한 허둥댐이 사라졌다. 엔지니어들이 제시간에 퇴근했다. 
이 방법들을 팀에 가르쳤다. 스터디를 진행했다. 실제 피드백을 바탕으로 접근법을 다듬었다. 하지만 내부에만 두었고, 동료들과만 공유했다. 

Until now.

지금까지는. 



## The Proof I Lived It (내가 직접 살아온 증거)
Some people ask: “Don’t Go Too Fast? Is that even possible in the real world?”
My answer is my career.
I worked at one company for 19 years out of my 25-year career. I served as research institute director for over 15 years.
In an era where developers change jobs every 2-3 years, I chose depth over breadth. I stayed and built. I refined and improved. I watched solutions evolve over decades, not quarters.

어떤 사람들은 묻는다: “Don’t Go Too Fast? 실제 세계에서 가능한가?” 
내 답은 내 커리어다. 
25 년 커리어 중 19년을 한 회사에서 일했다. 15 년 넘게 연구소장을 맡았다. 개발자들이 보통 2-3 년마다 이직하는 시대에, 나는 넓이보다 깊이를 선택했다. 머물면서 만들었다. 다듬고 개선했다. 분기가 아닌 수십 년에 걸쳐 솔루션이 진화하는 것을 지켜봤다. 

Why?
Because going properly beats going fast. Because depth beats breadth. Because the tortoise, in the end, beats the hare.
This book is the result of those 25 years. Not theory from an ivory tower, but wisdom forged in the daily reality of shipping software, meeting deadlines, and—most importantly—maintaining quality when everything pushed me to compromise.
When I say “Don’t Go Too Fast,” I’m not offering advice I read somewhere. I’m sharing how I’ve worked for a quarter century.

왜? 
제대로 가는 것이 빨리 가는 것을 이기기 때문이다. 깊이가 넓이를 이기기 때문이다. 결국 거북이가 토끼를 이긴다. 
이 책은 그 25 년의 결과물이다. 상아탑의 이론이 아니라, 소프트웨어를 출시하고, 마감을 맞추고—가장 중요하게—모든 것이 타협하라고 압박할 때 품질을 유지한 일상의 현실에서 단련된 지혜다. 
내가 “Don’t Go Too Fast”라고 말할 때, 어딘가에서 읽은 조언을 제공하는 것이 아니다. 
사반세기 동안 어떻게 일해왔는지를 공유하는 것이다. 

## Why Now? (왜 지금인가?)
For years, I was reluctant to publish these ideas.
Perhaps it was laziness. Perhaps it was the feeling that more refinement was needed.
Perhaps it was the classic engineer’s hesitation to share work that wasn’t “perfect.”
But recently, something changed.

수년간 이 아이디어를 출판하는 것을 꺼렸다. 
아마 게으름이었을 것이다. 아마 더 다듬어야 한다는 느낌이었을 것이다. 아마 “완벽”하지 않은 작업을 공유하는 것에 대한 전형적인 엔지니어의 망설임이었을 것이다. 
하지만 최근 무언가가 바뀌었다. 

In 2025, while developing an AI-based security solution, I’ve been collaborating intensively with AI tools. From direct experience, I’ve found that when you use AI well, you can achieve productivity similar to working with 3-4 developers.
But did this make DGTF unnecessary? The opposite. 
The more AI accelerates coding, the more important thoughtful design becomes. The faster AI builds things, the better you need to think about what to build. The more powerful the tools become, the more critical the judgment of the person using them.

2025년, AI 기반 보안 솔루션을 개발하면서 AI 도구와 집중적으로 협업하고 있다. 직접 경험해보니, AI 를 잘 다루면 3-4명의 개발자와 함께 일하는 것과 유사한 생산성을 확보할 수 있다. 
하지만 이것이 DGTF 를 불필요하게 만들었을까? 정반대다. 
AI 가 코딩을 가속화할수록, 신중한 설계가 더 중요해진다. AI 가 더 빨리 만들어줄수록, 무엇을 만들지 더 잘 생각해야 한다. 도구가 강력해질수록, 도구를 쓰는 사람의 판단이 더 중요해진다. 

The methodologies in this book are not threatened by AI—they become more essential.

이 책의 방법론은 AI 에 의해 위협받지 않는다—더 필수적이 된다. 

## What This Book Does NOT Solve (이 책이 해결하지 않는 것)
Let me be honest:
- It cannot fix toxic organizational culture
- It doesn’t work magically when you’re exhausted
- It won’t turn juniors into instant experts
- Quick reading cannot replace months of deliberate practice

I include this because honest limitations make honest methodology. The “Hard Questions” section in the Practical Guide addresses these limitations and how to cope with them in more detail.

솔직히 말하겠다: 
-  독성적인 조직 문화는 고칠 수 없다 
-  지쳤을 때 마법처럼 작동하지 않는다 
-  주니어를 즉시 전문가로 만들지 못한다 
-  빠르게 읽는 것으로 수개월의 연습을 대체할 수 없다 

이것을 포함하는 이유는, 솔직한 한계가 솔직한 방법론을 만들기 때문이다. 실전 가이드의 “어려운 질문들” 섹션에서 이러한 한계와 대응 방법을 더 자세히 다룬다. 


## What This Book Offers (이 책이 제공하는 것)
This book offers three things:
**First, theoretical foundation.** You’ll understand why ROD, TFD, and DGTF work, grounded in cognitive psychology, systems thinking, and innovation theory.
**Second, practical methodology.** You’ll get concrete, actionable techniques for what to do—not vague advice like “think better,” but checklists, workflows, and patterns you can use tomorrow.
**Third, proven results.** These aren’t laboratory ideas. They’re methodologies refined over decades in real projects, tested with real teams and real deadlines.

이 책은 세 가지를 제공한다: 
**첫째, 이론적 기반**. ROD, TFD, DGTF 가 왜 효과가 있는지, 인지심리학, 시스템 사고, 혁신 이론에서 뒷받침되어 이해하게 될 것이다. 
**둘째, 실용적 방법론**. 무엇을 해야 하는지 구체적이고 실행 가능한 기법을 얻게 될 것이다—“더 잘 생각해라” 같은 모호한 조언이 아니라 내일 사용할 수 있는 체크리스트, 워크플로우, 패턴. 
**셋째, 입증된 결과**. 이것들은 연구실 아이디어가 아니다. 수십 년에 걸쳐 현실 프로젝트에서 다듬어진 방법론으로, 현실의 팀과 현실의 마감에서 테스트되었다. 

## Who This Book Is For (누구를 위한 책인가)
This book is for:
- Senior developers who have burned out before
- Junior developers who can code but wonder why things work
- Technical leaders who need predictable outcomes
- Teams who want to build sustainable culture

이것은 다음 사람들을 위한 이야기이다: : 
-  바닥까지 태워본 적 있는 시니어 개발자 
-  코딩은 할 수 있지만 왜 작동하는지 궁금한 주니어 개발자 
-  예측 가능한 결과가 필요한 기술 리더 
-  지속 가능한 문화를 만들고 싶은 팀 

It’s for anyone seeking:
- Quality maintenance under pressure
- Reduced technical debt
- Predictable development schedules
- Sustainable pace without last-minute crises

이것은 다음을 원하는 사람들을 위한 이야기이다: 
-   압박 속에서도 품질을 유지하고 싶은 사람
- 기술 부채를 줄이고 싶은 팀
-   예측 가능한 개발 일정을 만들고 싶은 조직
-   막판 위기 없이 지속 가능한 속도로 일하고 싶은 개발자


## A Personal Note (개인적인 말)
Writing this book, I thought a lot about my past self. The junior developer struggling to learn quickly and prove himself. The manager frustrated watching his team burn out and bugs multiply.
If only I had known this earlier.
That’s why I’m publishing. To reach across the barrier of awareness. To speak to myself 25 years ago, and to you who are looking for the same thing sooner.
Our industry celebrates “fail fast, fail often.” But that’s not the only way. You can move thoughtfully, build it right the first time, construct rather than destroy.
Going slow is faster. Going thoughtful wins. The tortoise wins.
Let’s begin. 

이 책을 쓰면서 과거의 나 자신을 많이 생각했다. 막내 개발자로서 서둘러 무언가를 배우고 자신을 증명하려고 힘들어하던 때. 매니저로서 팀의 과로와 버그 급증을 보며 좌절하던 때. 
내가 더 일찍 이것을 알았더라면. 
그래서 출판하는 것이다. 인식의 벽 너머에 손을 내밀기 위해. 25 년 전의 나 자신에게, 그리고 같은 것을 더 빨리 찾고 있는 당신에게 말하기 위해. 
우리 산업은 “빠른 실패, 자주 실패”를 찬양한다. 하지만 그것이 유일한 길은 아니다. 신중하게 움직이고, 한 번에 올바르게 만들고, 부수기보다 건설할 수 있다. 
느리게 가는 것이 더 빠르다. 신중하게 가는 것이 승리한다. 거북이가 이긴다. 
시작하자. 




# About the Author

## Bakmeon Kim (김백면)

Software Engineer & Development Philosophy Practitioner 

소프트웨어 엔지니어 & 개발 철학 실천가 

Bakmeon Kim has been building software for over 25 years.
He began his career as a developer in 2000, eventually leading research institutes and serving as CTO across multiple companies. His experience spans enterprise solutions, AI platforms, smart factories, AR/VR applications, and security systems, with projects delivered in Korea, Taiwan, Japan, and the United States.

김백면은 25 년 넘게 소프트웨어를 만들어왔다. 
2000년에 개발자로 시작하여, 연구소장과 CTO 를 거쳤다. 그의 경험은 기업 솔루션, AI 플랫폼, 스마트팩토리, AR/VR 애플리케이션, 보안 시스템에 걸쳐 있으며, 한국, 대만, 일본, 미국에서 프로젝트를 수행했다. 

What sets Bakmeon apart is his commitment to depth over breadth. In an industry where developers typically change jobs every 2-3 years, he spent 19 years at a single company—building, refining, and perfecting his craft. He served as Research Institute Director for over 15 years, leading teams and conducting software engineering studies for more than a decade.
His philosophy is simple but profound: **good software is not software with many features, but software where features can be easily added**. This focus on maintainability—what he calls “change responsiveness”—has been the driving force
behind his work.

김백면을 차별화하는 것은 넓이보다 깊이에 대한 헌신이다. 개발자들이 보통 2-3 년마다 이직하는 산업에서, 그는 한 회사에서 19 년을 보냈다—만들고, 다듬고, 자신의 기술을 완성하면서. 15 년 넘게 연구소장으로 팀을 이끌며 10 년 이상 소프트웨어 공학 스터디를 
진행했다. 그의 철학은 단순하지만 심오하다: 좋은 소프트웨어는 기능이 많은 소프트웨어가 아니라, 기능을 쉽게 추가할 수 있는 소프트웨어다. 유지보수성에 대한 이 집중—그가 “변화 대응력”이라 부르는 것—이 그의 작업의 원동력이었다. 


## Professional Highlights (주요 경력)
- 25+ years in software development
소프트웨어 개발 25년 이상 
- 19 years at one company, demonstrating long-term commitment
한 회사에서 19년 근무, 장기적 헌신의 증거 
- 15+ years as Research Institute Director
15년 이상 연구소장 역할 수행 
- 10+ years leading software engineering studies
10년 이상 소프트웨어 공학 스터디 리딩 
- 100+ projects delivered across multiple industries
여러 산업에 걸쳐 100개 이상 프로젝트 수행 
- International project experience in Taiwan, Japan, and USA
대만, 일본, 미국 해외 프로젝트 경험 


## Industries Experienced (경험한 산업 분야)
- SCM (Supply Chain Management) systems
SCM (공급망 관리) 시스템 
- MES-based smart factory platforms
MES 기반 스마트팩토리 플랫폼 
- AI-powered demand forecasting/quality inspection/predictive maintenance
AI 기반 수요예측/품질검사/예지보전 
- AR/VR industrial metaverse
AR/VR 산업용 메타버스 
- AI-based security solutions (current)
AI 기반 보안 솔루션 (현재) 

## Areas of Expertise (전문 분야)

### Enterprise Solutions (기업 솔루션) :
-  SCM (Supply Chain Management) systems
SCM (공급망 관리) 시스템
-  MES (Manufacturing Execution Systems)
MES (제조 실행 시스템) 
-  Smart Factory platforms
스마트팩토리 플랫폼 

### AI & Emerging Technology (AI & 신기술) :
- Deep learning for demand forecasting
수요 예측을 위한 딥러닝 
-  Computer vision for quality inspection
품질 검사를 위한 컴퓨터 비전 
-  LLM/sLLM applications
LLM/sLLM 애플리케이션 
-  AR/VR industrial metaverse solutions
AR/VR 산업 메타버스 솔루션 

### Software Engineering (소프트웨어 공학) :
- Test-first development culture
테스트 우선 개발 문화 
-  Clean architecture and design patterns
클린 아키텍처와 디자인 패턴 
-  Team education and mentoring
팀 교육과 멘토링 
-  Development process optimization
개발 프로세스 최적화 

## Current (현재) (2025~) 
Currently developing an AI-based security solution, applying ROD, TFD, and DGTF principles to network security.
From direct experience, he has found that when you use AI well, you can achieve productivity similar to working with 3-4 developers. But this doesn’t diminish the importance of DGTF—it makes it more essential.  

AI 기반 보안 솔루션을 개발 중이다. 네트워크 보안에 ROD, TFD, DGTF 원칙을 적용하고 있다. 
AI 를 잘 다루면 3-4 명의 개발자와 함께 일하는 것과 유사한 생산성을 확보할 수 있다는 것을 직접 경험하고 있다. 하지만 이것이 DGTF 의 중요성을 줄이지 않는다—오히려 더 
중요하게 만든다. 

## Origin of the Principles (원칙의 시작)
In 1992, upon entering university, he felt behind among classmates who already knew how to program. He made two commitments then: 
> “Never stop thinking.” “Never stop being curious.”

1992년 대학 입학 당시, 이미 프로그래밍을 할 줄 아는 동기들 사이에서 뒤처짐을 느꼈다. 그때 다짐한 두 가지: 
> “절대 생각함을 멈추지 말자” “절대 궁금해함을 멈추지 말자” 

These commitments became the foundation of this book 30 years later. 

이 다짐이 30년 후 이 책의 기반이 되었다. 


## Education (학력)
- M.S. in Computer Science (Software Engineering focus) Soongsil University, Seoul, Korea
컴퓨터공학 석사 (소프트웨어 공학 전공) 숭실대학교, 서울 
- B.S. in Computer Science Soongsil University, Seoul, Korea
컴퓨터공학 학사 숭실대학교, 서울 

## Philosophy (철학)
“I hate doing the same thing twice. If I have to do something more than three times, I automate it.”
“Good software is not software with many options. Good software is software where options can be easily added.”
“Never stop thinking. Never stop being curious.”
These principles, established during his university years and refined over decades of practice, form the foundation of the ROD, TFD, and DGTF methodologies presented in this book.

“같은 일을 두 번 하는 것을 싫어한다. 세 번 이상 해야 한다면, 자동화한다.” 
“좋은 소프트웨어는 옵션이 많은 소프트웨어가 아니다. 옵션을 쉽게 추가할 수 있는 소프트웨어다.” 
“생각을 멈추지 마라. 호기심을 멈추지 마라.” 
대학 시절 확립되고 수십 년의 실천을 통해 다듬어진 이 원칙들이 이 책에서 소개하는 ROD, TFD, DGTF 방법론의 기반이다. 

## Personal (개인)
Bakmeon lives in Seoul, Korea, with his wife—whom he met in a high school English conversation club—and their daughter.
When not coding, he enjoys:
-  Roasting coffee at home
-  Playing clarinet (learning since 2015)
-  Woodworking at local workshops
-  Leather crafting
-  3D printing
-  Vocal lessons (since 2023)
-  Studying psychology through distance learning

He quit smoking in 2013 and now enjoys occasional drinks with his daughter.

김백면은 고등학교 영어 회화 동아리에서 만난 아내, 그리고 딸과 함께 서울에 살고 있다. 
코딩하지 않을 때는: 
-  집에서 커피 로스팅 
-  클라리넷 연주 (2015 년부터 배움) 
-  지역 공방에서 목공 
-  가죽 공예 
-  3D 프린팅 
-  보컬 레슨 (2023 년부터) 
-  방송대에서 심리학 공부 

2013년에 담배를 끊었고, 지금은 가끔 딸과 술을 즐긴다. 

## Connect (연락처 )
• GitHub: https://github.com/bakmeon/dont-go-too-fast
• Email: stillblueist@naver.com

This book represents 25 years of learning, practicing, and teaching. It is written not from theory alone, but from the daily reality of building software that lasts. 

이 책은 25년간의 배움, 실천, 가르침을 담고 있다. 이론만이 아니라, 지속되는 소프트웨어를 만드는 일상의 현실에서 쓰여졌다. 

Remember: Don’t Go Too Fast