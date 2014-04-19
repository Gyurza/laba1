laba1
=====
//Лаба 1. Пашкевич Егор БГУ-ММФ-5 группа-1 курс
//Написать программу, реализующую небольшую базу данных. Она должна содержать ввод
//данных с клавиатуры, форматированную печать в виде таблицы (в псевдографическом ис-
//полнении), чтение и запись базы данных на диск, сортировку по каждому полю записей в
//обоих порядках. Выбор действия осуществляется через текстовое меню. Варианты записей
//в базе данных:Адресная книга: фамилия, адрес, телефон

#include <iostream>
#include <fstream>
using namespace std;

struct adrkn{
	char fam[30];
	char adr[30];
	char tel[30];
};
typedef int(*fptr)(adrkn c, adrkn mid,  int T, int i);

void vvod(adrkn *c, int&n);
void vivod(adrkn *c, int n);
void sv(adrkn *c, int n);
void sor(adrkn *c, int n);
void mergeSort(adrkn *c, int l, int r, int s);
int sort1(adrkn c, adrkn mid, int T, int i){
	int j=0;
	if(i){
	if(strcmp(c.fam,mid.fam)<0)
        {
		j=1;
	}

		}
	return j;
}
int sort2(adrkn c, adrkn mid, int T, int i){
	int j=0;
	if(i){
	if(strcmp(c.fam,mid.fam)>0){
		j=1;
	}
	}
	return j;
}
int sort3(adrkn c, adrkn mid, int T, int i){
	int j=0;
	if(i){
	if(strcmp(c.adr,mid.adr)<0){
		j=1;
	}
	}
	return j;
}
int sort4(adrkn c, adrkn mid, int T, int i){
	int j=0;
	if(i){
	if(strcmp(c.adr,mid.adr)>0){
		j=1;
	}
	}
	return j;
}

void vvod(adrkn *c, int&n){
	cout<<"Familiya: ";
 cin>>c[n].fam;
 cout<<endl;
 cout<<"Adres: ";
 cin>>c[n].adr;
 cout<<endl;
 cout<<"Telefon: ";
 cin>>c[n].tel;
 cout<<endl;
 ++n;
}
void vivod(adrkn *c, int n){
	int j;
	cout<<"*************************************************"<<endl;
  cout<<"(";
 cout.width(15);
 cout<<"Familiya";
 cout.width(1);
 cout<<"|";
 cout.width(15);
 cout<<"Adres";
 cout.width(1);
 cout<<"|";
 cout.width(15);
 cout<<"Telefon";
 cout.width(1);
 cout<<")"<<endl;
 for( j=0;j<n;++j){
 cout<<"*************************************************"<<endl;
 cout<<"(";
 cout.width(15);
 cout<<c[j].fam;
 cout.width(1);
 cout<<"|";
 cout.width(15);
 cout<<c[j].adr;
 cout.width(1);
 cout<<"|";
 cout.width(15);
 cout<<c[j].tel;
 cout.width(1);
 cout<<")"<<endl;
 }
 cout<<"*************************************************"<<endl;
}
void sv(adrkn *c, int n){
 fstream fs;
 fs.open("adrkn.txt",ios::out|ios::binary);
 fs.write((char *)&n, sizeof(int));
 for(int j=0;j<n;++j){
 fs.write((char *)&c[j], sizeof(adrkn));
 }
 fs.close();
}
void sor(adrkn *c, int n){
	int k;
	static fptr table[4]={sort1, sort2, sort3, sort4};
	cout<<endl<<"1. Sortirovat\' po familii !!!priamoi poryadok!!!"<<endl<<"2. Sortirovat\' po familii !!!obratnii poryadok!!!"<<endl;
	cout<<"3. Sortirovat\' po adresy!!!pryamoi!!!"<<endl<<"4. Sortirovat\' po adresy!!!obratnii!!!"<<endl;
	cout<<"5. Ne sortirovat\'"<<endl;
	cin>>k;
	if(k==5){
		cout<<"Net tak net"<<endl;
	}
	else{
	mergeSort(c, 0, n, k);
	}
}

void mergeSort( adrkn *c, int l, int r, int s){
	int i, j, n, k, m;
	adrkn A[30], B[30];
	static fptr table[4]={sort1, sort2, sort3, sort4};
	if(l<r){

		mergeSort(c, l, (r+l)/2, s);
		mergeSort(c, (l+r)/2+1, r, s);
		for(i=0, m=l;i<=(r+l)/2; ++i, ++m){
			A[i]=c[m];
		}
		for(j=(l+r)/2+1, n=0;j<=r;++j,++n){
			B[n]=c[j];
		}
		i=(r+l)/2-l+1;
		n=r-(r+l)/2;
		j=0, k=0, m=l;
		while((j<i)&&(k<n)){
			if(table[s-1](A[j], B[k], 0 , 1)){
				c[m]=A[j];
				++j;
			}
			else{
				c[m]=B[k];
				++k;
			}
			++m;
		}
		while(j<i){
			c[m]=A[j];
			++j;
			++m;
		}
		while(k<n){
			c[m]=B[k];
			++k;
			++m;
		}
	}
}
