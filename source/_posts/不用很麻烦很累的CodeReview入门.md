---
title: 不用很麻烦很累的CodeReview入门
date: 2024-06-22 08:57:16
tags: 
    - "CodeReview"
    - "代码质量"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/code_review.jpg"
---

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/code/code1.png)

> 注：本文仅关注 Code Review 的实行，不讨论 “为什么要 Code Review” 或 “Code Review 是否有价值” 这类话题。

通过 Code Review 来保证项目代码质量、提高团队的技术水平，已经是很多公司的常规操作，大多数人也已经认可了进行 Code Review 的必要性。然而，现实是这样的：

”伴随着号召和口号，大家热情高涨，一顿操作花式输出，目指用洞察世界的双眼来拯救这个腐朽的Repo，而待多巴胺褪去，热情难以为继，全都恢复成了没有感情的 Coder，三天打鱼两天晒网，再到最后完全流于形式，直至热情再一次被唤醒……“

陷入循环中，我们不禁要呐喊：

## “为什么Code Review这么麻烦这么累”

### 1. 错的不是我，是这个PR
Code Review 会让人觉得麻烦、代码看起来很累，很多情况下确实是因为这个 PR 并不易于 Review，本身存在一些问题：

- PR 太大：动辄几千行代码、几十个文件修改，打开一看就怕了怕了，没有做好心理建设、没有两三个小时的空闲时间谁敢 Review，告辞告辞；
- PR 要做事的太多：东边儿全局修改了变量名，西边儿重构了公用方法，南边儿调整了页面样式，北边儿还顺便修了一个陈年老 bug……嚯，好家伙，一个迭代的需求一个 PR 就提上来了，也就 Author 勉强在当天还能搞清楚哪段是哪段，Reviewer 一路看下来就得怀疑人生；
- PR 没有上下文：“为什么要删除这段代码？为什么要加这么多分支逻辑？这么多类似的方法真的是必须的吗？这种场景没有判断，是遗漏了还是业务需求？”你是否有很多问号？？？总会有很多问题，在缺乏上下文时，是很难给出解答的，不写任何 Description，想要让 Reviewer 自己主动去搞清楚业务背景实在是不厚道，所以也别怪没人肯 Review 了；
- PR 代码质量太差：不遵守既定规范、格式乱七八糟、命名天马行空、逻辑晦涩难懂，整篇代码毫无“可读性”可言，对于这种 PR，Review 两眼是情分，直接Request Change才是本分。

### 2. 即使我有错，也是有原因的

如果，PR 确实不存在问题，代码整洁、逻辑清晰，Description 安排地明明白白，却还是没能得到回应，那的确应该是 Reviewer 的问题，不过为他们考虑考虑，可能也是有原因的：

- 工作量过于饱和：再简洁明了的 PR，Review 起来还是需要时间的，忙得已经没有喝水的时间，哪还有工夫去做 Code Review；
- 道不同不相为谋：你有你的原则，我有我的风格，既然谁也说服不了谁，那就这样吧；
- 技术领域不同：“我说另请高明吧，我实在也不是谦虚，我一个后端开发怎么就点进前端PR了呢？”；
- 看不出问题：虽然说起来有点尴尬，但确实是一个很普遍的情况，没有掌握方法、相关技术储备不足、Review 角度不同等等，一篇 PR 读下来，似乎觉得哪里不对，又似乎也有道理，Comment 憋不出来、Approve 又不放心，那就跳过跳过，等其他大牛出手吧。

那么，有这么多严峻的问题存在，你看，我们还有机会吗？

问得好！还记得文章的标题吗：

## “我们不用很麻烦很累就可以 Code Review”

### 1. 作为 Author

- PR 尽可能小
  - 创建 Draft PR：便于获得早期反馈，避免在代码成型后出现大规模的修改返工
  - PR 只关注一件事：降低阅读 PR 时的认知成本、更易于确定合理的 Assignee、Rollback 的可行性更高、开发更具计划性
  -  严格执行 Rebase：避免引入不必要的修改或造成 Review 的返工
- 添加关键的上下文
  - 不可缺少的 Description（必要时可尝试使用Github PR template辅助规范化），可以主要关注以下几点：
    - 这段代码改动的目的是？（通常情况下提供需求 Ticket 标题及链接即可）
    - 能预料可能会提出的问题，请提前说明（如存在多种近似解决方案，选择当前方案的原因）
    - （若存在）特定逻辑需要特定 Reviewer 关注，请提前说明
    - （若存在）批量修改，请提前说明
    - （若存在）UI 修改，请提供屏幕截图或可查看到实际效果的链接
  - 易于理解的 Commit Message，严厉抵制 ”fix bug“ 这种偷懒 message
  - 代码中添加必要的注释
- Assign给正确的人
  - Assign组内成员
  - 期望能给出反馈的人:
    - 擅长/熟识该业务领域的人；
    - 擅长/熟识该技术领域的人；
  - 期望能知晓该 PR 的人：
    - 会受到该代码改动影响的人；
    - 近期正在学习该业务/技术领域的人；
- 收到Comment及时跟进
  - 修改后及时回复 comment 或 refresh assignee，不要指望其他人能一直蹲守你的更新或修改
  - 出现意见分歧，避免过度依赖 Comment 交流，线下沟通优先，其次是 IM
  - 若因时间或计划问题无法在当前 PR 中完成修改，达成一致后创建 Tech Debt Ticket 备忘
  - 尽可能避免在已经Approve的PR上进行无关修改，以免给Reviewer造成不必要的重复工作

一句话总结：对自己提出的每一个 PR 负责

### 2. 作为Reviewer

- 主动阅读 Description
  - 作为普通 Reviewer，有必要了解 PR 的上下文
  - 作为业务/技术领域相关者，有必要了解代码及逻辑变更情况
- 得当的 Comment
  - 表述简洁清晰无歧义
  - 通常应具有目的性：明确知道自己想要的回复是哪一种，是希望 Author 改正问题？补充上下文？解答疑惑？或是说明计划？目的不明的 Comment 只会造成不必要的反复沟通
  - 不仅可以指出问题，同样可以请教问题
  - 给出肯定或表扬的 Comment
  - 无需过于正式：Comment 的数量并不会用于反映工作量，因而任何形式不会对双方工作造成影响的 Comment 都是可行且值得鼓励的
- Comment 后及时跟进
  - 及时回复 Author 的追问
  - Author 修改后及时 Resolve 或 Approve
- 及时给出反馈
  - 根据实际情况，给出 Approve，Comment 或 Request Change
  - 谨慎给出 Approve：Review 代码后，若有足够把握，可以给出 Approve；若存在疑虑（不了解相关业务、底层逻辑），也应给出 Comment 表明 Review 完成，不应让 Author 无止尽的空等
- 不要成为 Blocker
  - 仅有的 Repo Code Owner、点名指定的 Reviewer（业务/技术领域相关者），必须要给出反馈
  - Request Change 的 Reviewer，必须要即使跟进

一句话总结：对自己给出的每一个 Comment 和 Approve 负责

### 3. 参与者应有的共识

- Code Review 的主要目的不应是为了发现 Bug，也不应是为了检查代码风格和规范
- Code Review 是沟通的一种形式，作用是相互的
- Code Review 时不要吝啬你的赞美之词
- Code Review 时不可固执己见
- Code Review 时应正面且友善

最后，

## 还有几句要说：

看到这儿大家应该都能感觉到，Author 要做的事情和 Reviewer 要做的事情基本都是相辅相成，两边缺一不可，说到底，推动 Code Review 需要整个 Team 甚至公司一起努力，但需要努力，并不意味着“不用很麻烦很累”是 Fake News，只要 Code Review 成为公司的一种文化、一种习惯，所有的一切都会变得理所当然，且有趣。
