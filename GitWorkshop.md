# Git workshop



Git predstavlja readonly bazu

3 bitne stvari za git:

1. Working directory - direktorijum u koji smještamo naše fajlove (direktorijum u kojem se nalazi .git folder)
2. Index - priprema za git repo (ovdje smještamo naše izmjene)
3. Repository - commit-ovani fajlovi idu u repository



**git init** - kreira git repozitorijum i Index (u njemu git čuva sve svoje stvari - ne treba čaprkati po njemu)

Nakon inicijalizacije git prati sve promjene u folderu koji sadrži .git folder

**git status** - prikazuje trenutno stanje gita (šta je u WD, šta u Index)

​	untracked files - postoje u WD, ali ne u Indexu

​	changes to be committed - postoje u Indexu - trebaju se preslikati u Repository

**git add** zima.txt - smješta file iz WD u Index (korpa) - biramo koje fajlove hoćemo da slikamo. Biramo koje fajlove dodajemo u Index

**git commit -m "dodana nova pjesmica o zimi"** - smješta file iz Indexa u Repository (izvršili smo slikanje)

**git config --global user.email** prikazuje email (nalog) koji se koristi za rad sa gitom

**clear** - čisti konzolu

Sad smo napravili sliku sistema koji se prezentuje kao commit. Commit se prezentuje kao tačka. Svaki commit ima svoj hash (*cf23214*, *fea4a65*, ...).

Svaki commit se nadovezuje na prethodni (tačka koja predstavlja novi commit se spaja sa prethodnim commitom tako što napišemo strelicu prema prethodnom commitu)

**git log** --oneline

Svi commiti se smještaju na granu.
Imamo commit koji ide na granu (branch). Osnovna grana je **master**

Grana = pokazivač na commit 

HEAD - pokazivač na granu - određuje na koju granu se kači novi commit

Napravi se novi commit, automatski se pomjeri pokazivač na master, a samim tim i HEAD. Svaki sljedeći commit rezultuje pomjeranjem mastera i HEADa

**git reset** omogućava nam da se vratimo na prethodni commit

**git commit --amend -m "Promijenjena poruka"** Ako smo pogriješili samo zadnji commit 

​		--amend nam omogućava da izmijenimo posljednji commit

U gitu nema izmjena. Samo dodajemo. Garbage collector briše te "izmijenjene" commite

Nikada ne smijemo raditi amend nečega što smo push-ali



Postoje 3 varijante git reset-a:

1. git reset --mixed - čuva u WD 
2. git reset --hard - briše iz Indexa i iz WD
3. git reset --soft - čuva fajlove iz WD i u Indexu

git reset nam omogućava da se vratimo na prethodne commite

Hard briše sve. Razlika između mixed i soft je to što u soft moramo uraditi add kako bismo dodali u index. 

Po default-u radi se **mixed**

git reset --soft fea4[a65] - ne moramo čitav hesh kucati, dovoljno je 4 karaktera

git reset f7437b7 - vratili smo se na commit na kojem smo bili prije soft reset

reset komanda nam mijenja pokazivače, a tip reseta nam određuje šta će biti sa WD i Index-om tokom promjene pokazivača

git reset Head proljece.txt - uklanja iz Indexa



**git branch** branchName - kreira novi branch (novi pokazivač koji se zove branchName) koji pokazuje na isti commit kao i prethodni branch

**git checkout** branchName - prebacuje pokazivač HEADa na branchName (mijenja i WD ako postoje razlike u fajlovima)

git log --online --all 

**a dog** akronim za često korištene komande kod logovanja

​	--all

​	--decorate

​	---oneline

​	--graph



Spajanje različitih brenčeva: git merge 2x vrste, git rebase

git merge fast-forward (defualtni)

Kod merganja se moramo prebaciti na onu granu na kojoj hocemo da ostanemo nakon merga

**git merge** rock

Fast-forward se radi po default-u (može se raditi jer se rock nastavlja od master-ovog zadnjeg commita)

git merge --no-ff rock -m "spojen rock u master" (izbjegava fast forward)

Mergovanjem ne brišemo branch, mi možemo nastaviti commite na onom brenchu koji smo spojili (rock)

**git branch -d** rock - brise pokazivac na branch (commiti sa tog brancha ostaju)

​	git branch rock fda6793 - vraća pokazivač na istu poziciju sa koje smo ga obrisali

Postoji potencijalni problem kod brisanja brancha ako on nije spojen, jer ga onda GC može obrisati

Nema čekanja, ako je na nekom branchu problem, kreiramo novi branch na kojem mi nastavljamo raditi, kad se problem popraviit možemo uraditi **merge** ili **rebase **

git checkout pop

**git rebase** master - baza pop brancha postaje master

Branching strategija

Kao i kod *amenda* rebase pravi nove comite (prethodni commiti ostaju da vise, GC ce ih obrisati)



### Konflikt

Dešava se samo prilikom merge-ovanja ili rebase-ovanja

#### Konflikt u merge-u

git merge rock -m "rock u master" - dolazi do konflikta (u gitu će pisati master|MERGING)

​	Mi moramo ručno riješiti konflikte (odabrati koje izmjene sa kojeg brancha ćemo prihvatiti) - napraviti 	ispravan file. **Nakon rješavanja konflikta mi moramo testirati kod prije nego što postavimo na git jer git ne zna šta nama predstavlja konflikt** 

Nakon rješavanja konflikta: git add ., git commit i tjt

#### Konflikt u rebase-u

Kod merge-a dolazimo na granu na koju hocemo da dodamo (prebacujemo se na maste), kod rebase-a je obrnuto (na rock, pa spajamo na master). Nakon što riješimo konflikt moramo izvršiti **git rebase --continue**

git checkout rock
git rebase master 
git am --show-current-patch - da vidimo šta je bilo problematično
git add . 
git rebase --continue



### Remote (Git Workshop day 2)

Remote predstavlja server na koji mi pušamo sadržaj working directory-a
origin/master se ne može checkout-ovati

**git init --bare** - umjesto u .git folder smješta sve fajlove iz .git u folder u kojem se nalazimo

**git clone** *path* ( ~/Desktop/gitovanje/fakeserver/fakeproject.git) pravi git folder i u njega smiješta sav source code

**git push** - šalje commit na git server (u našem primjeru to je folder fakeserver koji glumi ulogu GitHub,BitBucket i sl.)

Pored obične master grane, imamo i origin/master.
Grane se sinhronizuju sa origin/master (grana na remote-u)
Kada uradimo commit prije git push komande, pomjerice se pokazivač grane na kojoj radimo, ali origin/master tada kasni za commitom.
Tek kada uradimo *git push* onda će origin/master pokazivati na najnoviji pushovani commit.

Sad drugi zaposleni mora da uradi **git fetch**, prije nego što počne vršiti neke izmjene. 

**git fetch** - dohvati trackovanu granu (origin/master)

**git pull = git fetch & git merge**

Ako sad uradimo git pull opet će se izvršiti git fetch pa onda git merge (možemo fetchovati koliko god puta hoćemo)

git merge origin/master

Problem kod *git pull* je što ne znamo kakav merge ćemo dobiti. Merge ide logikom, ako može fast-forward onda to uradi, u suprotnom moramo mi ručno mergati

Merge pull startegija 

Možemo podešavati pull i merge strategiju u git config fileu

Git game 👉 <https://learngitbranching.js.org/>

**git branch -vv -a** 

**git push --set-upstream origin rock**  - dodaje origin na novi branch (rock)

Google: checkout remote branch (umjesto git checkout orgin/rock staviti git checkout rock ili git checkout --track origin/rock)

Git nije backup code-a, source controla omogućava da lakše upravljamo u kodu
Firme imaju svoju strategiju kako se radi source control (git flow, trunk based i sl.)

#### Git flow

​	feature/nazivFeatura

Te strategije omogućavaju bolji kvalitet koda. Možemo lakše pregledati kod kroz različite commite.
Svaki commit treba biti smislen. 

Dobra konvencija prilikom commita:

1. Smislen naslov
2. Summary (-uradio a, -uradio b i sl.)
3. Opis (šta je urađeno - ne treba pisati romane)

GitWorkshop 3  👉01:20:00 

#### Trunk based

Problem kod grana je što se može desiti da master branch ode predaleko, dok mi radimo na našoj grani, tada plaćamo branch credit. Bolje je malo po malo raditi (fetch & rebase).

Integracija koda

Branching strategije

Feature flagging - pusti se kod i dio aplikacije koja ne radi i nije vidljivo korisniku (ranije integrišemo i onda dovršavamo)

<https://www.pluralsight.com/>

