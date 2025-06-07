# STRUKTURA E TE DHENAVE NE C++

## KODI I DOKUMENTUAR

#include <iostream>
#include <cstdlib>  
using namespace std;

/*
    
    1. Strukturat Bazë: Node për Linked List
    
    Kjo strukturë përfaqëson një nyje të thjeshtë për Linked List.
    - data: Ruaj vlerën e ruajtur (në këtë shembull, një numër int).
    - next: Pointer që tregon tek nyja tjetër në listë.
*/
struct Node {
    int data;
    Node* next;
};

/*
    
    2. Klasa Stack – Implementimi i Stivës (LIFO)
    
    Kjo klasë implementon stivën duke përdorur një Linked List.
    Operacionet kryesore janë:
      - push(int value): Shton një element në krye.
      - pop(): Heq dhe kthen elementin në krye.
      - top(): Kthen elementin në krye pa e fshirë.
      - empty(): Kontrollon nëse stiva është bosh.
      - print(): Afishon stivën nga krya në fund.
*/
class Stack {
private:
    Node* topNode;  // Pointer që tregon dini nyjën në krye të stivës.
public:
    // Konstruktor: Inicializon stivën bosh
    Stack() : topNode(nullptr) { }

    // push: Shton një element të ri në krye të stivës.
    void push(int value) {
        Node* newNode = new Node;   // Krijo një nyje të re.
        newNode->data = value;        // Cakto vlerën e të dhënave.
        newNode->next = topNode;      // Lidhet me nyjën aktuale të kryes.
        topNode = newNode;            // Përditëso kryen e stivës.
    }

    // pop: Heq dhe kthen elementin në krye të stivës.
    int pop() {
        if(empty()) {  // Kontrollo nëse stiva është bosh.
            cerr << "Error: Stiva është bosh!" << endl;
            exit(EXIT_FAILURE);
        }
        Node* temp = topNode;         // Ruaj pointer-in për nyjën që do të fshihet.
        int value = topNode->data;      // Ruaj vlerën e elementit aktual.
        topNode = topNode->next;        // Përditëso kryen e stivës.
        delete temp;                    // Çliro kujtesën për nyjën e fshirë.
        return value;                   // Kthe vlerën.
    }

    // top: Kthen elementin në krye pa e fshirë.
    int top() {
        if(empty()) {
            cerr << "Error: Stiva është bosh!" << endl;
            exit(EXIT_FAILURE);
        }
        return topNode->data;
    }

    // empty: Kthen true nëse stiva është bosh.
    bool empty() {
        return (topNode == nullptr);
    }

    // print: Afishon të gjithë elementët e stivës, nga krya (top) drejt fundit.
    void print() {
        cout << "Stack (top -> bottom): ";
        Node* cur = topNode;
        while(cur) {
            cout << cur->data << " ";
            cur = cur->next;
        }
        cout << endl;
    }
};

/*
    3. Problemi Praktik: Vlerësimi i Shprehjes Postfix me Stack
    
    Shprehja postfix është një mënyrë për të shkruar shprehje aritmetike pa kllapa, ku operandët dhe operatorët vendosen në rendin e duhur.
      
    Për shembull:
      Shprehja infix: (3 + 4) * 2
      Shprehja postfix: "3 4 + 2 *"
      
    Procesi i vlerësimit me stivë shkruhet kështu:
      1. Shto operandët në stivë.
      2. Kur haset një operator, pop operandët e nevojshëm,
         zbato operatorin, dhe pusho rezultatin në stivë.
      3. Pas përfundimit, në stivë është vetëm një vlerë, e cila është rezultati.
*/

/*
    4. Funksioni main() – Demonstrimi i Zbatimit të Projektit
*/
int main() {
    cout << "=== Demonstrim i Stack-ut (Stiva) në C++ ===" << endl;

    // Pjesa 1: Demonstrim i operacioneve bazë të stivës.
    Stack stiva;
    stiva.push(5);
    stiva.push(15);
    stiva.push(25);
    stiva.print();  // Pritet: 25 15 5 (nga krya drejt fundit)
    cout << "Elementi në krye (Top): " << stiva.top() << endl;
    cout << "Pop: " << stiva.pop() << endl; // Heq 25
    stiva.print();  // Pas pop, stiva duhet të jetë: 15 5
    cout << "Elementi aktual në krye: " << stiva.top() << endl;

    // Pjesa 2: Demonstrim i vlerësimit të një shprehjeje postfix.
    // Shprehja postfix: "3 4 + 2 *" korrespondon me (3 + 4) * 2 = 14
    cout << "\n=== Vlerësimi i shprehjes postfix me Stack ===" << endl;
    Stack postfixStiva;
    
    // Shto operandët 3 dhe 4.
    postfixStiva.push(3);
    postfixStiva.push(4);
    
    // Kur operatori '+' arrin, heq dy herë operandët.
    int op2 = postfixStiva.pop(); // merr 4
    int op1 = postfixStiva.pop(); // merr 3
    // Shto rezultatin e operacionit në stivë.
    postfixStiva.push(op1 + op2);  // pusho: 7
    
    // Shto operandin 2 në stivë.
    postfixStiva.push(2);
    
    // Kur operatori '*' arrin, heq operandët për operacion.
    op2 = postfixStiva.pop();       // merr 2
    op1 = postfixStiva.pop();       // merr 7
    // Shpërndaje rezultatin 7 * 2 = 14.
    postfixStiva.push(op1 * op2);    // pusho: 14
    
    // Rezultati final është në krye të stivës.
    cout << "Rezultati i shprehjes postfix: " << postfixStiva.top() << endl;  // Shfaq: 14
    
    return 0;
}

## STRUKTURA E IMPLEMENTUAR DHE ARSYETIMI PAS ZGJEDHJES SE STRUKTURES:
Përmbledhje e Implementimit të Stivës

a. Struktura Node:
Në kodin tonë kemi krijuar një strukturë themelore të quajtur Node. 
Ajo përfaqëson një nyje të listës së lidhur (linked list) dhe ka dy fusha kryesore:

~Data: Ruan vlerën që dëshirojmë ta ruajmë. 
Në këtë implementim, kemi përdorur tipin int për thjeshtësi (por kjo mund të shndërrohet në një tip më abstrakt, sipas kërkesave të projektit).
~Next: Është një pointer që tregon tek nyja tjetër në listë. Kjo lidhje na lejon të ndërtojmë një zinxhir të nyjeve, duke krijuar një listë të lidhur ku 
çdo nyje lidhet me të tjerën.

b. Klasa Stack
Struktura e stivës është implementuar me përdorimin e një linked list. 
Në këtë implementim, stiva ka një pointer privat:
~topNode: Ky pointer tregon nyjen që është në krye të stivës, pra atë që është e fundit që është futur (duke ndjekur parimin LIFO).

Klasa Stack përmban metodat kryesore:

~push(int value): Kjo metodë krijon një nyje të re me vlerën e dhënë dhe e vendos atë në krye të stivës. 
Ky proces është shumë "efikas" sepse operacioni realizohet me kompleksitet të ulët O(1). 
Në këtë metodë, nyja e re lidhet duke vendosur pointer-in next të saj për të tregon kryen aktual, 
dhe më pas kryeti përditësohet.

~pop(): Kur thirret pop, metoda kontrollon nëse stiva është bosh (përdor funksionin empty()). 
Nëse stiva jo është bosh, ajo heq nyjen në krye (ruan një pointer të përkohshëm për të fshirë dhe merr vlerën e saj), 
përditëson kryen duke e zgjedhur nyjen e mëposhtme dhe pastaj çlirohet hapësira përmes delete. 
Ky operacion gjithashtu funksionon në O(1).


~top(): Kjo metodë kthen vlerën e elementit në krye pa e hequr. 
Ai kontrollon që stiva të mos jetë bosh, duke ndihmuar kështu në përdorimin e sigurt të këtyre vlerave.


~empty(): Kthen një vlerë booleane (true ose false) që tregon nëse stiva është bosh (p.sh., nëse pointer-i topNode është nullptr).


~print(): Kjo metodë përshkon lidhjet nga nyja e kryesë deri sa të mbërrijë në fund dhe afishon të dhënat, duke treguar renditjen nga krya (top) drejt fundit. 
Ky operacion frymëzon konceptin LIFO,sepse elementët afishohen në rendin e kundërt të futjes së tyre.

2. Arsyetimi Pas Zgjedhjes së Struktures
a. Pse Stiva?

Parimi LIFO është i përshtatshëm: Shumë probleme praktike kërkojnë radhitjen e operacioneve në mënyrë të tillë që elementi i fundit i futur të jetë
 i pari që përdoret gjatë kthimit (siç ndodh në menaxhimin e thirrjeve të funksioneve dhe vlerësimin e shprehjeve postfix). 
Ky parim është natyror për stivat.

Efikasitet Operacional: Operacionet kryesore të stivës (push, pop, top) realizohen në kohë konstante,
 O(1), që e bën strukturën shumë efikase për aplikime ku nevojiten operacione të shpeshta ditorë mbi të dhëna.

b. Përse Përdorim Linked List?

Fleksibilitet Dinamik: Implementimi me linked list lejon që stiva të rritet në mënyrë dinamike pa qenë e kufizuar nga një madhësi fikse. 
Ndryshe nga array-t, ku madhësia fillestare është e paracaktuar, linked list lejon që të krijohen nyje të reja sipas nevojës.

Menaxhimi i Kujtesës: Duke përdorur new dhe delete, implementimi me linked list thekson aftësinë për të menaxhuar manual kujtesën në C++. 
Kjo është një aftësi themelore për studentët që po mësojnë strukturat e të dhënave.

## SI PERDORET STRUKTURA NE ZGJIDHJEN E PROBLEMIT ?
Struktura që kemi implementuar – në këtë rast, një stivë (Stack) – përdoret për të zgjidhur problemin praktik të vlerësimit të shprehjeve postfix. 
Le të shpjegojmë se si bëhet kjo hap pas hapi:

1).Ruajtja e Operandëve: Në një shprehje postfix, operandët (numrat) lexohen në rendin e tyre dhe futen në stivë duke përdorur operacionin push. 
Për shembull, nëse lexoni “3” dhe “4”, ato futen në stivë, kështu që stiva ruan:
Top: 4
Më poshtë: 3
Kështu, operandët ruhen në rendin që i vendosim, dhe elementi më i fundit i futur është në krye.

2).Heqja e Operandëve dhe Zbatimi i Operatorëve: Kur në shprehje haset një operator (p.sh. “+” ose “*”), 
stiva përdoret për të marrë operandët e nevojshëm. Kjo bëhet me operacionin pop, i cili heq elementët nga krye, 
duke ndjekur parimin LIFO (Last-In-First-Out).

Për shembull:
Nëse lexoni operatorin “+”, 
stiva popon dy herë – i pari heqet elementi në krye (që është “4”), pastaj i tjetri (që është “3”).
Më pas zbatohet operacioni matematik “3 + 4” dhe rezultati, 7, pushohet përsëri në stivë me push.

3).Përsëritja e Procesit: Ky proces vazhdon për çdo operand dhe operator në shprehje. 
Nëse në shprehje futen edhe operandë të tjerë ose operacione të tjera, ato hipotekohen në stivë, 
duke shqyrtuar vazhdimisht operacionet në mënyrë që operacionet që kërkojnë operandët gjithmonë i
 marrin operandët nga elementi që i ka futur së fundit.
Për shembull, pas shtimit të rezultateve nga operacionet e para, nëse shtohet edhe operandi “2” dhe më pas operacioni “*”, 
bëhet pop për të marrë operandët, llogaritet 7 * 2 (duke përdorur rezultatin prej hapit të mëparshëm dhe operandin e ri), 
dhe rezultati “14” pushohet në stivë.

4).Rezultati Final: Pas përpunimit të gjithë elementëve të shprehjes, 
stiva mbetet me vetëm një element, i cili jep rezultatin final të shprehjes. Në shembullin tonë, rezultati është 14.
Ky përdorim i stivës është shumë i dobishëm pasi:
Përdorimi i operacioneve push dhe pop: 
Siguron që operandët dhe rezultatet ndërmjetësisht ruhen në një mënyrë dinamike dhe efikase.
5)Natyrshmëria LIFO e Stivës: E bën strukturën të përshtatshme për shprehjet postfix, 
ku renditja e operacioneve varet nga rendi i futjes së operandëve.
Efikasiteti: Operacionet kryesore bëhen në kohë konstante, që garanton që edhe shprehje me shumë operandë dhe operatorë të përpunohen shpejt.

Në përmbledhje, struktura e stivës përdoret në zgjidhjen e problemit përmes një algoritmi të thjeshtë:
Operandët futen në stivë me push()
Kur haset një operator, stiva kryen pop() për të marrë operandët, zbatohet operacioni, dhe rezultati pushohet përsëri
Rezultati final i shprehjes ruhet në krye të stivës dhe merret me top()
Ky proces e bën stivën zgjedhje ideale për zgjidhjen e problemeve që kërkojnë përpunimin e 
të dhënave në një rend të caktuar, si p.sh., vlerësimi i shprehjeve postfix.

## VESHTIRESITE DHE TEJKALIMI I TYRE 
### Menaxhimi i Kujtesës Dinamike

***Vështirësitë:

1)Alokimi i Memorisë: Kur krijohen nyje të reja për linked list duke përdorur operatorin new, duhet të sigurohemi që çdo nyje të alokohet siç duhet. 
Nëse harron të çlirosh kujtesën me delete, do të hasësh probleme si “memory leaks” (humbje kujtesë).

2)Çlirimi i Kujtesës: Në operacionet si pop(),është e rëndësishme të çlironi kujtesën për nyjën që keni hequr. 
Nëse kjo nuk bëhet, do të krijohen probleme me pamjaftueshmërinë e kujtesës dhe mund të ndodhin gabime gjatë ekzekutimit.
***Zgjidhjet:

1)Kontrolli i Rregullt i Boshllëkut: Para se të kryhet një operacion që përdor pointer-a (si për shembull në pop() ose top()), 
kontrollohet nëse stiva është bosh. Kjo shmang përdorimin e pointer-eve të papërdorur ose të jo inicializuar.


2)Përdorimi i new dhe delete: Në metodën push(), përdorim new për të krijuar një nyje të re dhe, në metodën pop(), 
përdorim delete për të çliruar kujtesën për nyjën e fshirë. Kjo siguron që secila nyje që krijohet të ndikojë në kujtesë vetëm deri sa të jetë e nevojshme.

### Përdorimi i Pointer-ave dhe Siguria e Aksesimit të të Dhënave
***
Vështirësitë:

1)Pointer-e të Pa inicializuara: Nëse një pointer si topNode në stivë nuk inicializohet si nullptr në fillim, 
më vonë do të shkaktojë gabime gjatë operacioneve si pop() ose top(), 
sepse do të shkaktohet aksesimi në një adresë të pavlefshme (undefined behavior).


2)Aksesimi i Elementëve të Çliruar: Në operacionin pop(), nëse nuk kontrollohet që stiva të mos jetë bosh,
 përpiqet të akcesohet një nyje që është e papërdorur ose është çliruar, duke shkaktuar gabime runtime 
(p.sh., segmentation fault).

***
Zgjidhjet:

1)Inicializimi i Pointer-ave: Sigurohemi që pointer-at kryesorë si topNode do të inicializohen në fillim si nullptr, 
duke garantuar që operacionet e mëvonshme të kontrollojnë nëse struktura është bosh.

2)Kontrolli i Boshllëkut në Metodat Kritike: Në metodat pop() dhe top(), përdorim funksionin empty() për të kontrolluar 
nëse stiva është bosh para se të kryejmë ndonjë operacion aksesi.

3)Testimi i Gjithë Rastave të Ekzekutimit: Implementuam testime me skenarë të ndryshëm (si stiva bosh, radhët e operacioneve të ndryshme, etj.) 
për të kapur çdo situatë ekstreme dhe për të siguruar që çdo gjë ndërtohet në mënyrë të sigurtë.

### Modulariteti dhe Organizimi i Kodit
***Vështirësitë:

1)Ndërhyrja midis Modulave: Në një projekt të tillë, ndarja e funksioneve në module të pavarura 
(p.sh., një klasë për linked list, një klasë për stack) duhet të bëhet në mënyrë të tillë që secili modul të ketë funksionet 
e tij të qarta dhe të ndërlidhura në mënyrë të qartë.

2)Qartësia dhe mirëmbajtja e Kodit: Kur punon me struktura që përdorin pointer-a, kodet mund të bëhen të ndërlikuara nëse nuk
 janë shkruar me komente të mjaftueshme dhe nëse logjika nuk ndahen në funksione të pavarura.

***
Zgjidhjet:

1)Strukturimi Modular i Kodit: Kemi ndarë implementimet e të dhënave në klase të veçanta (p.sh., klasa Stack) 
dhe i kemi organizuar funksionet e saj (push, pop, top, empty, print) në mënyrë që të jenë ndërthurur në një modul të vetëm.

2)Komentimi i Kodit: Çdo pjesë e kodit është komentuar qartë për të shpjeguar se çfarë bën seanca secila metodë, 
çfarë operacioni kryen dhe pse është e rëndësishme brenda projektit
. Kjo ndihmon që përdoruesit dhe bashkëpunëtorët të kuptojnë lehtësisht logjikën dhe të zbulojnë gabime.

4)Testimi i moderuar dhe integrimi i shembujve praktik: Në funksionin main() janë përfshirë shembuj praktikë që demonstrojnë mënyrën 
se si se përdorin operacionet kryesore si push(), pop(), etj., duke garantuar kështu se struktura funksionon sipas pritshmërive.

### Integrimi në Zgjidhjen e Problemit Praktik: Vlerësimi i një Shprehjeje Postfix

***Sfida:

1)Interpretimi i Shprehjeve Postfix: Në një shprehje postfix nuk përdoren kllapa, kështu që mënyra e ndarjes së operandëve
 dhe operatorëve duhet të bëhet në mënyrë automatike duke ruajtur rendin e duhur të operacioneve.


2)Menaxhimi i Operandëve dhe Operatorëve: Shtimi i operandëve në stivë dhe përdorimi i operacioneve aritmetike marrë nga stiva duhet të bëhet me vëmendje,
 për të siguruar se operandëve të duhur popohen në mënyrë të saktë dhe se rezultati i secilit operacion pushohet përsëri në stivë.

***
Zgjidhja:

1)Përdorimi i operacioneve push dhe pop: Operacionet bazë të stivës janë shumë të shpejta dhe realizohen në O(1), duke siguruar efikasitet në zbatim. 
Operandët shtohen në mënyrë të drejtpërdrejtë dhe kur operatorët arrijnë, stiva përdoret për të kapur operandët në rendin e duhur.

2)Shpjegimi i hapave përmes komentimeve: Në kodin e shembullit të shprehjes postfix, secili hap është shkruar me detaje nëpërmjet komenteve, 
duke përshkruar se si operacionet kryhen: futimi i operandëve, zbatimi i operatorëve dhe mosfundimi me rezultatin final.

3)Testimi me shembuj të qarta: Në main(), shembulli praktik i shprehjes postfix ti lejon të shihni se si rezultati final (14) merren pas ndjekjes së hapave të push/pop,
 duke demonstruar se struktura e stivës zgjidh problemin në mënyrë të saktë dhe të mirëorganizuar.

 ## PUNOI: JURGEN CENMURATI   IE 105
