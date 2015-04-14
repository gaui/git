# Git leiðbeiningar á íslensku

Þessar leiðbeiningar eru miðaðar fyrir byrjendur sem vilja læra á Git. Ef þið sjáið villur eða viljið bæta einhverju við, endilega sendið PR *(Pull Request)*.

Ég er búinn að vera að halda utan um Git leiðbeiningar í PDF-formi [hér](http://gaui.is/stuff/Git.pdf) en ákvað að deila þessu og leyfa fólki að bæta við og breyta - og hvað er betra til þess en Git, GitHub og Markdown?

## Sækja Git

Hægt er að sækja Git fyrir öll helstu stýrikerfin á [http://git-scm.com](http://git-scm.com).

### Mac OS X

Þægilegasta leiðin til að setja upp Git á Mac (og halda utan um uppfærslur) er í gegnum [Homebrew](http://brew.sh), en það er tól sem gerir manni kleift að halda utan um pakka/tól fyrir Mac. Eftir að brew hefur verið sett upp þarf einfaldlega að keyra `brew install git`. Til að sækja uppfærslur keyrir maður `brew upgrade git`

## Hvað er Git?

Git er kerfi fyrir dreifða útgáfustjórn *(e. distributed revision control system)*. Ólíkt öðrum kerfum getur Git séð um útgáfustjórn án viðkomu servers. Önnur kerfi eins og SVN, TFS (TFVC), o.s.frv. eru miðstýrð *(e. centralized)* sem þýðir að útgáfustýringin er á miðlægum server.

Git er hugbúnaður sem búinn til var af Linus Torvalds, höfundi Linux stýrikerfisins. Linus bjó til Git til að sjá um útgáfustjórn fyrir Linux kernelinn, en gaf það svo út á endanum til almennings. Git er í dag vinsælasta kerfið til að sjá um útgáfustjórnun.

Fyrir þá sem vilja kynna sér Git undir húddinu geta lesið nánar [hér](http://wildlyinaccurate.com/a-hackers-guide-to-git/).

## Uppsetning

Eftir uppsetningu á Git fyrir Windows er opnað *Git Bash*, sem er skel fyrir Git. Ef Git er sett upp fyrir Linux/Mac er ekki þörf á *Git Bash*, þar sem notuð er skelin sem fylgir stýrikerfinu.

Áður en við byrjum er gott að setja inn nafn og netfang, sem fest verður við allar breytingar sem þú gerir.

Skipanirnar eru eftirfarandi:

    git config --global user.name "John Doe"
    git config --global user.email johndoe@example.com


## Nýtt repository

Til að byrja útgáfustjórn með Git er búin til svokölluð geymsla *(e. repository)*. Þetta er mappan sem inniheldur t.d. allan kóðann þinn sem þú vilt útgáfustýra.

Til að búa til nýtt repository ferðu í möppu sem þú vilt útgáfustýra og skrifar inn eftirfarandi skipun:

    git init

Þá er mappan orðin að Git repository. Flóknara er það ekki.

## Git högun

### Local vs. Remote

- Commit eru breytingar sem þú gerir *local* á þinni tölvu
 - *"Ég er búinn að committa breytingunum"*
- Push eru *local* breytingarnar þínar, færðar yfir á server/remote
 - *"Ég er búinn að pusha breytingunum"*

### Flæði

Flæðið er í stuttu máli eftirfarandi:

- Working Directory (Untracked)
- Staging Area (Index)
- Local Repository
- Remote Repository (origin)

![Git diagram](http://i.imgur.com/QZ8qeoZ.png)

Til að líkja Git saman við eitthvað í raunveruleikanum þá má líkja því saman við skrif á bók:

- **Working Directory (Untracked)**
 - Þú byrjar að skrifa á fullu og býrð til drög/uppköst sem þú ýmist vistar eða hendir.
- **Staging Area (Index)** - `git add`
 - Þú tekur saman það sem þú vilt gefa út og setur í umslag.
- **Local Repository** - `git commit`
 - Þú setur umslagið í kassa með einkvæmum merkimiða ásamt lýsingu, dagsetningu og höfundi verksins.
- **Remote Repository** - `git push`
 - Þú sendir kassann til ritstjórnar/útgefanda.

### SSH

Þegar átt eru samskipti við server (remote) er oft notað SSH. SSH samskipti eru dulkóðuð og mjög örugg.

Til þess að geta átt örugg samskipti við servera eins og t.d. GitHub þarf maður að auðkenna sig gagnvart honum. Gengið er í gegnum ferlið einu sinni (fyrir hverja tölvu) og er það eftirfarandi:

- [Búa til GitHub aðgang](https://github.com/join)
- [Búa til SSH lykla par](https://help.github.com/articles/generating-ssh-keys/#step-2-generate-a-new-ssh-key)
- [Tengja SSH public lykil við GitHub aðganginn þinn](https://help.github.com/articles/generating-ssh-keys/#step-4-add-your-ssh-key-to-your-account)

### Commit

Hvert commit í Git er með upplýsingar um hvernig repository'ið (Working Directory) lítur út á þeim tíma. Í hverju committi eru upplýsingar um höfund þess og dagsetningu. Hvert commit fær einkvæmt SHA1 hash gildi, eins konar kennimerki *(e. identifier)* fyrir committið.

Commit í Git eru með pointer á committið þar á undan *(parent commit*). Þannig er sagan uppbyggð í Git. Eins og sjá má á neðangreindri mynd.

![git commit](http://i.imgur.com/zJoIbzH.png)

### Branch

Brönch í Git eru bendar *(e. pointer)* á tiltekin commit. Git er allt byggt á pointerum.

![Pointers](http://i.imgur.com/SG1oyOQ.png)

Á myndinni hér fyrir ofan má sjá eftirfarandi:

- HEAD pointerinn bendir á master pointerinn
- master pointerinn bendir svo á C2 committið
- test pointerinn bendir á T1 committið
- C1 er fyrsta committið
- T1 committið var búið til út frá C2 (C2 er parent T1)
- C2 comittið var búið til út frá C1 (C1 er parent C2)
- Þegar þú gerir `checkout master` ertu á C2 committinu
- Þegar þú gerir `checkout test` ertu á T1 committinu

#### HEAD

Í Git er bendir *(e. pointer)* sem ber heitið *HEAD*. Þessi pointer færist í hvert skipti sem þú ferð á nýjan stað í repository'inu (notar `git checkout` skipunina). Þetta er því í raun bendir á þann stað sem þú ert staddur á núna.

#### Detached HEAD

Þetta er þegar *HEAD* pointerinn bendir ekki á tiltekið branch, heldur á tiltekið commit í Git.

Segjum að þú farir á committið á undan `master`, þá ertu kominn með detached HEAD.

## Skipanir

Hér fyrir neðan er yfirlit yfir helstu skipanir í Git.

### git init

Þetta býr til nýtt repository og er upphafið á öllu.

    Initialized empty Git repository in c:/Users/gaui/Desktop/test/.git/

### git status

Þetta er mest notaðasta skipunin í Git. Hún sýnir á hvaða branchi þú ert ásamt öllum breytingum sem hafa verið gerðar frá síðasta committi. Sýnir hvaða skrá eru untracked (ekki verið að fylgjast með), hvaða skrám hefur verið breytt/eytt ásamt því hvaða breytingar hafa verið *staged* (gerðar tilbúnar fyrir commit).

![git status](http://i.imgur.com/rWCZZMb.png)

Á myndinni hér fyrir ofan er:

- *baz.txt* breytt og sú breyting hefur verið staged (tilbúin fyrir commit).
- *bar.txt* eytt en sú breyting hefur ekki verið staged.
- *foo.txt* breytt sú breyting hefur ekki verið staged.
- *smuu.txt* bætt við, en Git er ekki að fylgjast með henni (untracked).

### git checkout

Í Git er bendir *(e. pointer)* sem ber heitið *HEAD*. Þessi pointer færist í hvert skipti sem þú ferð á nýjan stað í repository'inu - og til þess notaru þessa skipun.

#### git checkout `<branch>`

Færir þig yfir á branch og færir um leið *HEAD* pointerinn.

#### git checkout -b `<branch>`

Býr til nýtt branch og færir þig yfir á það.

### git add

Bæta við skrá/möppu í Staging Area (Index) þannig breytingin verði tilbúin fyrir commit. Eins og á myndinni hér fyrir ofan var *baz.txt* bætt við í Staging Area.

#### git add `<file>`

Tiltekinni skrá bætt við í Staging Area.

#### git add `.`

Öllum skrám/breytingum bætt við í Staging Area.

#### git add `-u`

Bæta við öllum eyddum skrám í Staging Area.

#### git add `-p`

Leyfir þér að velja hvaða breytingar þú vilt setja í Staging Area. Segjum að þú gerir þrjár breytingar í sömu skránni. Þú getur stage'að tveimur af þessum þremur breytingum í skránni.

### git reset

Þessi skipun skiptist í þrjá parta; mixed, soft og hard. Default er `--mixed`

#### git reset `--mixed`

Þetta er defaultið. Þetta endurstillir Staging Area (Index) og breytir ekki Working Directory.

Þetta er notað þegar þú vilt taka allar skrár úr Staging Area (Index).

#### git reset `--soft`

Þetta endurstillir ekki Staging Area (Index) og breytir ekki Working Directory.

#### git reset `--hard <commit>`

Þetta breytir Working Directory alveg eins og í committinu sem þú tilgreinir.

Þetta er notað mest til að bakka með allar breytingar sem þú hefur gert síðan síðasta commit.

Þetta er hættuleg aðgerð og getur haft alvarlegar afleiðingar í för með sér.

Ekki nota þetta ef þú ert búinn að pusha á server.

### git revert `<commit>`

Þessi skipun er notuð til þess að bakka með breytingar sem búið er að pusha á server. Hún býr til nýtt commit þar sem öllum breytingunum í committinu er snúið við. + verður - og - verður +

### git commit

Býr til nýtt commit í local repository með staged breytingum.

#### git commit `-m "Commit Message"`

Býr til nýtt commit í local repository með staged breytingum ásamt skilaboðunum `"Commit Message"`.

    $ git commit -m "Commit Message"
    [master f4b8f86] Commit Message
    2 files changed, 2 insertions(+), 2 deletions(-)

### git remote

Remote er vísun í eitthvað repository uppstreymi, til dæmis GitHub. Þetta gæti alveg eins verið vísun í aðra möppu á tölvunni þinni með sama repository. Git er alveg sama, svo framarlega sem það er Git repository.

#### git remote `add origin git@github.com:user/repo.git`

Bætir við nýrri remote vísun sem ber heitið `origin` og vísar á repository á GitHub.

Þetta þarf ekki að heita `origin`, gæti þess vegna verið `github`. `origin` er bara venja í Git og stendur fyrir uppruna repository'sins.

#### git remote `-v`

Sýnir öll remote í núverandi repository.

### git clone `<repository>`

Klónar Git repository í nýja möppu.

![git clone](http://i.imgur.com/TQJ5CHu.png)

Á myndinni hér fyrir ofan klónaði ég repository undir slóðinni *C:\Users\gaui\Desktop\test* yfir í nýja möppu, sem ber heitið *test_clone*. `ls` skipunin sýnir bara hvaða skrár urðu til.

Ef *test* repositoryið væri með langa sögu af breytingum, myndi klónaða repository'ið innihalda alla þá sögu. Þannig ef þú klónar repository á GitHub færðu alla breytingasöguna yfir til þín.

### git push

#### git push `origin <branch>`

Pushar local branchi yfir á `origin` remote repository'ið.

Eins og í dæminu hér að ofan vísar `origin` á `git@github.com:user/repo.git` þannig við myndum senda allar breytingarnar þangað.

#### git push `origin :<branch>`

Eyðir branchi á `origin` remote repository'inu. Taktu eftir tvípunktinum á undan branch nafninu.

### git pull `origin <branch>`

Sækir allar breytingar frá `origin` remote repository'inu.

Eins og í dæminu hér að ofan vísar `origin` á `git@github.com:user/repo.git` þannig við myndum sækja allar nýjustu breytingarnar þaðan.

### git log

Sýnir lista yfir öll commit á núverandi branchi.

#### git log `<branch>`

Sýnir lista yfir öll commit á ákveðnu branchi.

#### git log `<branch1>..<branch2>`

Sýnir lista yfir öll commit sem eru í *branch2* en ekki í *branch1*.

#### git log `<branch1>...<branch2>`

Sýnir lista yfir commit sem eru annað hvort í *branch1* eða *branch2*.

Hér fyrir neðan má sjá mynd sem útskýrir þetta betur:

![git log](http://i.imgur.com/vM7PtCn.png)

### git diff

Sýnir breytingar frá síðasta committi (þær breytingar sem þú ert að vinna í).

#### git diff `...<branch>`

Sýnir mun/diff á committum á branchi, sem eru ekki á branchinu sem þú ert á núna *(HEAD)*.

#### git diff `<branch1>...<branch2>`

Sýnir mun/diff á tveimur local brönchum. Sýnir þær breytingar sem eru í *branch2* en ekki *branch1*.

Sýnir þér mun milli *"merge base"* þessara tveggja brancha og endans á *branch2*.

Hér fyrir neðan má sjá mynd sem útskýrir þetta betur:

![git diff](http://i.imgur.com/1PfbKPU.png)

#### git diff `--staged`

Sýnir allar breytingar sem eru í Staging Area (Index).

### git branch

Sýnir lista yfir öll local brönch.

#### git branch `--remote`

Sýnir lista yfir öll remote brönch.

#### git branch `--all`

Sýnir lista yfir öll brönch (local og remote)

#### git branch `<branch>`

Býr til nýtt local branch.

#### git branch `-d <branch>`

Eyðir local branchi sem hefur verið merge'að inn í master.

#### git branch `-D <branch>`

Eyðir local branchi sem hefur *EKKI* verið merge'að inn í master.

### git rm

#### git rm `<file>`

Eyðir skrá úr Git. Eyðir bæði úr Working Directory og stage'ar breytingunni. Þú þarft svo að committa breytingunni.

#### git rm `--cached <file>`

Eyðir skrá úr Git. Stage'ar breytingunni en eyðir skránni ekki úr Working Directory.

### git clean `-f -d`

Eyðir untracked skrám úr Working Directory.

`-f` = Force `-d` = Directories

### git merge `<branch>`

Merge'ar branch inn í *current branch* sem þú ert á.

Til að sjá hvaða branch þú ert á skaltu nota `git status` skipunina.

### git prune `origin --dry-run`

Sýnir öll local brönch sem passa ekki við remote brönch.

`--dry-run` = Bara sýna, ekki breyta neinu.

### git tag

Býr til pointer á tiltekið commit. Þetta er svipað og branch.

#### git tag v1.0

Býr til pointerinn `v1.0` á committið sem þú ert staddur á.

## Aliases

Hér fyrir neðan eru gagnlegir aliasar (sérsniðnar skipanir) sem hægt er að nota.

### git unpushed

Sýnir öll commit sem ekki er búið að pusha.

    git config --global alias.unpushed "log --branches --not --remotes"

### git lg

Sýnir öll commit á fallegu formi (með branch grafi).

    git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

### git tags

Sýnir öll tög og hvenær þau voru búin til.

    git config --global alias.tags "log --tags --simplify-by-decoration --pretty='format:%ai %d'"

### git branches

Sýnir öll brönch í röð eftir því hvenær var síðast committað á þau.

    git config --global alias.branches "for-each-ref --sort=-committerdate refs/heads/ --format='%(committerdate:short) %(authorname) %(refname:short)'"

### git recent

Sýnir brönch sem nýbúið er að vinna á.

    git config --global alias.recent "for-each-ref --count=10 --sort=-committerdate refs/heads/ --format='%(refname:short) %(committerdate:relative)'"

### git stats

Sýnir yfirlit yfir fjölda committa og nöfn höfunda.

    git config --global alias.stats "shortlog -s -n"

### git conflicts

Sýnir skrár sem eru í *conflicted state*.

    git config --global alias.conflicts "diff --name-only --diff-filter=U"
