BEST Bucuresti vrea sá trimità o echipã de 2 delegati la un eveniment international, dar ei trebuie sa facã parte din departamente diferite. Vi se vor oferi perechi cu oamenii ce fac parte din acelasi department.
Determinati câte echipe de delegati se pot alege.
Exemplu
numar de voluntari = 4
perechile sunt [1, 2] si [2, 3]
Asta inseamnã cá sunt 4 persoane care vor sã plece (de la 0 la 3). 1 si 2 sunt in acelasi department, dar 2 este in acelasi department si cu 3, deci 1, 2 si 3 fac parte dintr-un singur department, iar O este într-unul diferit.
Existà 3 echipe posibile: [0, 1], [0, 2] sau [0, 3].
Input Format
Prima linie contine douã numere întregi n si t, numarul de voluntari si numárul de perechi.
Fiecare dintre rândurile urmatoare contine numere întregi separate prin spatiu care denot departamentele din care fac parte.
Constraints
• 1 <= n <= 10^5
・1<=t<= 10^5
Output Format
Un singur întreg, numarul de echipe posibile.
Important
1. Pe prima linie a codului trebuie sã fie un comentariu ce contine numele echipei
2. Puteti rezolva in orice limbaj de programare

REZOLVARE DIANA
using namespace std;

int main(){

int n,t,a[100000]={0};

int k=1, x, y;

cin >>n>>t;

for (i=0; i<t ; i++）{

cin>>x>>y; 

if(a[x]==0 && a[y]==0) [

a[x]=k; 

a[y]=k;

k++;

}

else if (a[x] !=0){

a[y]=a[x];

}

else{

a[x]=a[y];

}
}

int perechi=0;

for(int i=0;i<n:i++)
{

if(a[i]==0){

a[i]=k;

k++;

}

}

for(int i=0;i<n;i++)


for(int j=i+1; j<n;j++){


if(a[i]!=a[j]) perechi++;

}

cout<<perechi;

return 0;

}



