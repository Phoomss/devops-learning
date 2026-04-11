# Git Workflow / ขั้นตอนการทำงานกับ Git

## What is Git? / Git คืออะไร?

Git is a distributed version control system for tracking code changes and enabling collaboration. It allows multiple developers to work on the same project simultaneously without overwriting each other's work.

Git คือระบบควบคุมเวอร์ชันแบบกระจาย ใช้สำหรับติดตามการเปลี่ยนแปลงของโค้ดและเปิดใช้งานการทำงานร่วมกัน Git ช่วยให้นักพัฒนาหลายคนสามารถทำงานในโปรเจกต์เดียวกันได้พร้อมกันโดยไม่เขียนทับงานของกันและกัน

## Key Concepts / แนวคิดหลัก

- **Repository (ที่เก็บรหัส)**: A project folder tracked by Git. It contains all project files, directories, and the complete history of changes.
  - **ที่เก็บรหัส (Repository)**: โฟลเดอร์โปรเจกต์ที่ Git ติดตาม ประกอบด้วยไฟล์ทั้งหมด ไดเรกทอรี และประวัติการเปลี่ยนแปลงทั้งหมด

- **Commit (คอมมิต)**: A snapshot of changes at a specific point in time. Each commit represents a saved state of the project.
  - **คอมมิต (Commit)**: ภาพรวมของการเปลี่ยนแปลง ณ จุดเวลาใดเวลาหนึ่ง แต่ละคอมมิตแสดงถึงสถานะที่บันทึกของโปรเจกต์

- **Branch (สาขา)**: An independent line of development. Branches allow you to work on features without affecting the main codebase.
  - **สาขา (Branch)**: เส้นทางการพัฒนาที่เป็นอิสระ สาขาช่วยให้คุณทำงานบนฟีเจอร์ได้โดยไม่กระทบกับโค้ดหลัก

- **Merge (รวม)**: The process of combining branches together. It integrates changes from one branch into another.
  - **รวม (Merge)**: กระบวนการรวมสาขาร่วมกัน มันผสานการเปลี่ยนแปลงจากสาขาหนึ่งไปยังอีกสาขาหนึ่ง

- **Remote (รีโมท)**: A version of your repository hosted on another server (e.g., GitHub, GitLab). It enables collaboration with other developers.
  - **รีโมท (Remote)**: เวอร์ชันของที่เก็บรหัสของคุณที่โฮสต์อยู่บนเซิร์ฟเวอร์อื่น (เช่น GitHub, GitLab) ช่วยให้ทำงานร่วมกับนักพัฒนาคนอื่นได้

- **Clone (โคลน)**: A complete copy of a repository, including all files and history. Used to get a local working copy.
  - **โคลน (Clone)**: สำเนาของที่เก็บรหัสทั้งหมด รวมถึงไฟล์และประวัติทั้งหมด ใช้เพื่อรับสำเนางานมาทำงานในเครื่อง

## Common Git Commands / คำสั่ง Git ที่พบบ่อย

### Basic Operations / การดำเนินการพื้นฐาน

```bash
git init                    # Initialize a new repository / เริ่มต้นที่เก็บรหัสใหม่
git clone <url>             # Clone an existing repository / โคลนที่เก็บรหัสที่มีอยู่
git add .                   # Stage all changes for commit / เตรียมการเปลี่ยนแปลงทั้งหมดเพื่อคอมมิต
git commit -m "message"     # Commit staged changes with a message / คอมมิตการเปลี่ยนแปลงที่เตรียมพร้อมพร้อมข้อความ
git push origin main        # Push changes to remote repository / ดันการเปลี่ยนแปลงไปยังที่เก็บรหัสรีโมท
git pull                    # Fetch changes from remote and merge / ดึงการเปลี่ยนแปลงจาก รีโมทและรวม
git status                  # Show current state of working directory / แสดงสถานะปัจจุบันของไดเรกทอรีทำงาน
git log                     # Show commit history / แสดงประวัติคอมมิต
```

### Branching / การจัดการสาขา

```bash
git branch feature          # Create a new branch named 'feature' / สร้างสาขาใหม่ชื่อ 'feature'
git checkout feature        # Switch to the 'feature' branch / สลับไปยังสาขา 'feature'
git checkout -b feature     # Create and switch to 'feature' in one command / สร้างและสลับไปยัง 'feature' ในคำสั่งเดียว
git merge feature           # Merge 'feature' branch into current branch / รวมสาขา 'feature' เข้ากับสาขาปัจจุบัน
git branch -d feature       # Delete the 'feature' branch (after merge) / ลบสาขา 'feature' (หลังจากรวมแล้ว)
git branch -a               # List all branches (local and remote) / แสดงรายการสาขาทั้งหมด (ในเครื่องและรีโมท)
```

### History / ประวัติ

```bash
git log --oneline           # Show compact commit history (one line per commit) / แสดงประวัติคอมมิตแบบย่อ (หนึ่งบรรทัดต่อคอมมิต)
git log --graph --all       # Show visual graph of all branches and merges / แสดงกราฟภาพของสาขาและการรวมทั้งหมด
git diff                    # Show unstaged changes / แสดงการเปลี่ยนแปลงที่ยังไม่ได้เตรียม
git diff HEAD~1             # Show diff with the previous commit / แสดงความแตกต่างกับคอมมิตก่อนหน้า
git blame file              # Show who changed which line and when / แสดงว่าใครเปลี่ยนบรรทัดไหนและเมื่อไหร่
```

## Git Workflow Models / โมเดลขั้นตอนการทำงานกับ Git

### 1. Trunk-Based Development / การพัฒนาแบบ/trunk

**How it works / วิธีการทำงาน:**
- Single main branch (trunk/main)
  - มีสาขาหลักเพียงสาขาเดียว (trunk/main)
- Developers merge small changes frequently (multiple times per day)
  - นักพัฒนารวมการเปลี่ยนแปลงเล็กๆ บ่อยๆ (หลายครั้งต่อวัน)
- Feature flags hide incomplete features
  - ธงฟีเจอร์ (feature flags) ซ่อนฟีเจอร์ที่ยังไม่สมบูรณ์
- Continuous delivery from trunk
  - ส่งมอบอย่างต่อเนื่องจาก trunk

**Workflow / ขั้นตอนการทำงาน:**
```
main: A → B → C → D → E (production / ใช้งานจริง)
         ↗     ↗
feature1 ──┘     │
feature2 ────────┘
```

**Pros / ข้อดี:**
- ✅ Faster integration / รวมโค้ดได้เร็วกว่า
- ✅ Fewer merge conflicts / ความขัดแย้งในการรวมมีน้อยกว่า
- ✅ Simpler workflow / ขั้นตอนการทำงานเรียบง่ายกว่า
- ✅ Better for CI/CD / เหมาะกับ CI/CD มากกว่า
- ✅ Always releasable / พร้อมปล่อยได้ตลอดเวลา

**Cons / ข้อเสีย:**
- ❌ Requires discipline (small commits) / ต้องมีวินัย (คอมมิตขนาดเล็ก)
- ❌ Feature flags needed / จำเป็นต้องใช้ feature flags
- ❌ Can break main if not careful / อาจทำให้ main พังได้ถ้าไม่ระวัง

**Best for / เหมาะที่สุดสำหรับ:**
- Teams practicing CI/CD / ทีมที่ฝึกใช้ CI/CD
- DevOps culture / วัฒนธรรม DevOps
- Web applications / เว็บแอปพลิเคชัน
- Continuous deployment / การส่งมอบอย่างต่อเนื่อง

### 2. Git Flow / ขั้นตอน Git Flow

**How it works / วิธีการทำงาน:**
- `main` branch (production / ใช้งานจริง)
- `develop` branch (integration / รวมโค้ด)
- Feature branches (`feature/*` / สาขาฟีเจอร์)
- Release branches (`release/*` / สาขาปล่อยเวอร์ชัน)
- Hotfix branches (`hotfix/*` / สาขาแก้ไขด่วน)

**Workflow / ขั้นตอนการทำงาน:**
```
main:    A ───────────────────── E (production / ใช้งานจริง)
          ↘                   ↗
develop:   B ─── C ─── D ────┘
              ↗         ↖
feature:     F ────────┘
```

**Process / กระบวนการ:**
1. Create feature branch from develop / สร้างสาขาฟีเจอร์จาก develop
2. Develop feature / พัฒนาฟีเจอร์
3. Merge to develop / รวมเข้ากับ develop
4. Create release branch / สร้างสาขาปล่อยเวอร์ชัน
5. Test and fix bugs / ทดสอบและแก้ไขบั๊ก
6. Merge to main and tag / รวมเข้ากับ main และติดแท็ก

**Branch Types / ประเภทของสาขา:**
```bash
# Feature / ฟีเจอร์
git checkout -b feature/new-feature develop  # Create feature branch from develop / สร้างสาขาฟีเจอร์จาก develop
git checkout develop                         # Switch to develop / สลับไป develop
git merge feature/new-feature                # Merge feature into develop / รวมฟีเจอร์เข้ากับ develop

# Release / ปล่อยเวอร์ชัน
git checkout -b release/1.0 develop  # Create release branch / สร้างสาขาปล่อยเวอร์ชัน
# Fix bugs / แก้ไขบั๊ก
git checkout main                    # Switch to main / สลับไป main
git merge release/1.0                # Merge release to main / รวม release เข้า main
git tag -a v1.0                      # Tag the release / ติดแท็กเวอร์ชัน
git checkout develop                 # Switch back to develop / สลับกลับ develop
git merge release/1.0                # Merge release to develop too / รวม release เข้า develop ด้วย

# Hotfix / แก้ไขด่วน
git checkout -b hotfix/urgent-fix main  # Create hotfix from main / สร้าง hotfix จาก main
# Fix bug / แก้ไขบั๊ก
git checkout main                       # Switch to main / สลับไป main
git merge hotfix/urgent-fix             # Merge hotfix to main / รวม hotfix เข้า main
git checkout develop                    # Switch to develop / สลับไป develop
git merge hotfix/urgent-fix             # Merge hotfix to develop too / รวม hotfix เข้า develop ด้วย
```

**Pros / ข้อดี:**
- ✅ Structured / มีโครงสร้างชัดเจน
- ✅ Clear release process / กระบวนการปล่อยเวอร์ชันชัดเจน
- ✅ Good for versioned software / เหมาะกับซอฟต์แวร์ที่มีเวอร์ชัน
- ✅ Separates development from production / แยกการพัฒนาออกจากการใช้งานจริง

**Cons / ข้อเสีย:**
- ❌ Complex / ซับซ้อน
- ❌ Merge conflicts common / ความขัดแย้งในการรวมเกิดขึ้นบ่อย
- ❌ Slower delivery / ส่งมอบช้ากว่า
- ❌ More branches to manage / มีสาขามากกว่าต้องจัดการ

**Best for / เหมาะที่สุดสำหรับ:**
- Traditional releases / การปล่อยเวอร์ชันแบบดั้งเดิม
- Versioned products / ผลิตภัณฑ์ที่มีเวอร์ชัน
- Mobile apps / แอปมือถือ
- Enterprise software / ซอฟต์แวร์องค์กร

### 3. GitHub Flow / ขั้นตอน GitHub Flow

**How it works / วิธีการทำงาน:**
- Single `main` branch / มีสาขา `main` เพียงสาขาเดียว
- Feature branches for changes / ใช้สาขาฟีเจอร์สำหรับการเปลี่ยนแปลง
- Pull Requests for review / ใช้ Pull Request สำหรับการรีวิว
- Deploy from main / ปล่อยจาก main

**Workflow / ขั้นตอนการทำงาน:**
```
main:    A ─────── C ────── E (production / ใช้งานจริง)
          ↗         ↗
feature:  B ────────┘
```

**Process / กระบวนการ:**
1. Create branch from main / สร้างสาขาจาก main
2. Make changes / ทำการเปลี่ยนแปลง
3. Open Pull Request / เปิด Pull Request
4. Review and discuss / รีวิวและอภิปราย
5. Merge to main / รวมเข้ากับ main
6. Deploy / ปล่อยใช้งาน

**Best for / เหมาะที่สุดสำหรับ:**
- GitHub/GitLab workflows / ขั้นตอนการทำงาน GitHub/GitLab
- Web applications / เว็บแอปพลิเคชัน
- Continuous deployment / การส่งมอบอย่างต่อเนื่อง
- Code review culture / วัฒนธรรมการรีวิวโค้ด

## Comparison Table / ตารางเปรียบเทียบ

| Aspect (ด้าน) | Trunk-Based | Git Flow | GitHub Flow |
|---|---|---|---|
| Branches (สาขา) | 1 main + short-lived / 1 main + สาขาอายุสั้น | 5+ types / 5+ ประเภท | 1 main + features / 1 main + ฟีเจอร์ |
| Merge frequency (ความถี่ในการรวม) | Multiple times/day / หลายครั้งต่อวัน | End of feature/sprint / สิ้นสุดฟีเจอร์/สปรินต์ | After PR review / หลังรีวิว PR |
| Release cadence (จังหวะการปล่อย) | Continuous / ต่อเนื่อง | Scheduled / กำหนดเวลา | On demand / ตามต้องการ |
| Complexity (ความซับซ้อน) | Low / ต่ำ | High / สูง | Medium / ปานกลาง |
| CI/CD fit (เหมาะกับ CI/CD) | Excellent / ยอดเยี่ยม | Moderate / ปานกลาง | Good / ดี |
| Team size (ขนาดทีม) | Any / ทุกขนาด | Larger teams / ทีมขนาดใหญ่ | Small-medium / เล็ก-ปานกลาง |
| Learning curve (ความยากในการเรียน) | Easy / ง่าย | Steep / ยาก | Moderate / ปานกลาง |

## Why the Difference Matters / ทำไมความแตกต่างจึงสำคัญ

**Choose Trunk-Based if / เลือก Trunk-Based ถ้า:**
- You want continuous deployment / คุณต้องการการส่งมอบอย่างต่อเนื่อง
- Your team is comfortable with feature flags / ทีมของคุณคุ้นเคยกับ feature flags
- You prioritize delivery speed / คุณให้ความสำคัญกับความเร็วในการส่งมอบ

**Choose Git Flow if / เลือก Git Flow ถ้า:**
- You need versioned releases / คุณต้องการการปล่อยเวอร์ชัน
- Multiple versions maintained simultaneously / ต้องดูแลหลายเวอร์ชันพร้อมกัน
- Regulatory/compliance requirements / มีข้อกำหนดด้านกฎระเบียบ/การปฏิบัติตาม

**Choose GitHub Flow if / เลือก GitHub Flow ถ้า:**
- You want simplicity / คุณต้องการความเรียบง่าย
- Code review is important / การรีวิวโค้ดสำคัญสำหรับคุณ
- You deploy frequently / คุณปล่อยใช้งานบ่อยครั้ง

## Best Practices / แนวทางปฏิบัติที่ดีที่สุด

1. **Commit often / คอมมิตบ่อยๆ**: Small, logical changes. Each commit should represent a single unit of work.
   - การเปลี่ยนแปลงขนาดเล็กและเป็นเหตุเป็นผล แต่ละคอมมิตควรแทนหน่วยงานเดียว

2. **Write good messages / เขียนข้อความที่ดี**: Clear, descriptive messages help everyone understand what changed and why.
   - ข้อความที่ชัดเจนและพรรณนาช่วยให้ทุกคนเข้าใจว่าอะไรเปลี่ยนไปและทำไม

3. **Use branches / ใช้สาขา**: Isolate work in progress. Never commit directly to main unless it's a trivial fix.
   - แยกงานที่กำลังดำเนินการ ห้ามคอมมิตเข้า main โดยตรง เว้นแต่เป็นการแก้ไขเล็กน้อย

4. **Review code / รีวิวโค้ด**: Pull requests before merge. Code review catches bugs and spreads knowledge.
   - ใช้ Pull Request ก่อนรวม การรีวิวโค้ดช่วยจับบั๊กและเผยแพร่ความรู้

5. **Keep main green / รักษา main ให้เขียว**: Never break production. Always run tests before merging.
   - ห้ามทำให้ production พัง รันทดสอบเสมอ ก่อนรวม

6. **Use .gitignore / ใช้ .gitignore**: Exclude unnecessary files (build artifacts, secrets, IDE configs).
   - ยกเว้นไฟล์ที่ไม่จำเป็น (artifact ที่ build แล้ว, ความลับ, การตั้งค่า IDE)

## Commit Message Convention / ข้อตกลงข้อความคอมมิต

```
# Format / รูปแบบ:
type: description
ประเภท: คำอธิบาย

# Types / ประเภท:
feat: add user authentication        # feat: ฟีเจอร์ใหม่ / เพิ่มระบบยืนยันตัวตนผู้ใช้
fix: resolve login bug               # fix: แก้ไขบั๊ก / แก้บั๊กเข้าสู่ระบบ
docs: update README                  # docs: เอกสาร / อัปเดต README
style: format code                   # style: รูปแบบโค้ด / จัดรูปแบบโค้ด
refactor: simplify logic             # refactor: ปรับโครงสร้างโค้ด / ลดความซับซ้อนของลอจิก
test: add unit tests                 # test: เพิ่มการทดสอบ / เพิ่ม unit tests
chore: update dependencies           # chore: งานเบ็ดเตล็ด / อัปเดต dependencies
```

## Resolving Merge Conflicts / การแก้ไขความขัดแย้งในการรวม

```bash
# When conflict occurs / เมื่อเกิดความขัดแย้ง
git status                    # See conflicted files / ดูไฟล์ที่มีความขัดแย้ง
# Edit files to resolve / แก้ไขไฟล์เพื่อแก้ไขความขัดแย้ง
git add resolved-file         # Mark as resolved / ทำเครื่องหมายว่าแก้ไขแล้ว
git commit                    # Complete merge / สำเร็จการรวม
```

**Steps / ขั้นตอน:**
1. Run `git status` to identify conflicted files / รัน `git status` เพื่อระบุไฟล์ที่มีความขัดแย้ง
2. Open each conflicted file and look for `<<<<<<<`, `=======`, and `>>>>>>>` markers / เปิดไฟล์ที่มีความขัดแย้งแต่ละไฟล์และมองหาเครื่องหมาย
3. Edit the file to keep the correct code / แก้ไขไฟล์เพื่อเก็บโค้ดที่ถูกต้อง
4. Run `git add <file>` to mark as resolved / รัน `git add <ไฟล์>` เพื่อทำเครื่องหมายว่าแก้ไขแล้ว
5. Run `git commit` to complete the merge / รัน `git commit` เพื่อสำเร็จการรวม

## Practice Exercises (Task 2) / แบบฝึกหัดปฏิบัติ (งานที่ 2)

1. **Compare workflows / เปรียบเทียบขั้นตอนการทำงาน**
   - Create a repository with Git Flow / สร้างที่เก็บรหัสด้วย Git Flow
   - Create a feature branch, merge it / สร้างสาขาฟีเจอร์ แล้วรวม
   - Create another repository with Trunk-Based Development / สร้างที่เก็บรหัสอีกที่ด้วย Trunk-Based Development
   - Compare the workflows / เปรียบเทียบขั้นตอนการทำงานทั้งสอง

2. **Document differences / บันทึกความแตกต่าง**
   - Why is Trunk-Based different from Git Flow? / ทำไม Trunk-Based ถึงแตกต่างจาก Git Flow?
   - When to use each approach? / เมื่อไหร่ควรใช้แต่ละแนวทาง?
   - What are the pros and cons? / ข้อดีและข้อเสียของแต่ละวิธีคืออะไร?

3. **Team simulation / จำลองการทำงานเป็นทีม**
   - Simulate multi-developer workflow / จำลองขั้นตอนการทำงานที่มีนักพัฒนาหลายคน
   - Practice merge conflict resolution / ฝึกแก้ไขความขัดแย้งในการรวม
   - Set up branch protection rules / ตั้งกฎการป้องกันสาขา

4. **Bonus / โบนัส**
   - Set up CI/CD with your workflow / ตั้งค่า CI/CD กับขั้นตอนการทำงานของคุณ
   - Configure branch rules (require PR reviews) / ตั้งค่ากฎสาขา (บังคับให้มีการรีวิว PR)
   - Use git hooks for pre-commit checks / ใช้ git hooks สำหรับการตรวจสอบก่อนคอมมิต

## Resources / ทรัพยากร

- [Pro Git Book (Free) / หนังสือ Pro Git (ฟรี)](https://git-scm.com/book/en/v2)
- [Git Flow (Original Article / บทความต้นฉบับ)](https://nvie.com/posts/a-successful-git-branching-model/)
- [Trunk Based Development (Official Site / เว็บไซต์ทางการ)](https://trunkbaseddevelopment.com/)
- [GitHub Flow Documentation / เอกสาร GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow)
- [Git Documentation / เอกสาร Git](https://git-scm.com/doc)
