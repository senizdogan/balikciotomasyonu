#include <iostream>
#include <fstream>
#include <cstring>
#include <conio.h>
#include <clocale>
#include <cstdio>

using namespace std;

struct Balik
{
    char balik_no[10];
    char balik_turu[15];
    float kilo;
    float kilo_fiyat;
    char avlanma_bolge[15];
};

void BalikEkle();
void BalikListele();
void BalikAra();
void BalikSil();
void BalikDuzenle();

int main()
{
    setlocale(LC_ALL, "Turkish");
    char secim;
    
    do
    {
        system("cls");
        cout << "1 - Balik Ekle\n";
        cout << "2 - Balik Listele\n";
        cout << "3 - Balik Ara\n";
        cout << "4 - Balik Sil\n";
        cout << "5 - Balik Duzenle\n";
        cout << "Seciminiz: ";
        cin >> secim;

        switch (secim)
        {
        case '1': BalikEkle(); break;
        case '2': BalikListele(); break;
        case '3': BalikAra(); break;
        case '4': BalikSil(); break;
        case '5': BalikDuzenle(); break;
        }

        cout << "\nDevam icin a, cikis icin c: ";
        secim = getche();

    } while (secim == 'a' || secim == 'A');

    return 0;
}
void BalikEkle()
{
    ofstream yaz("Balik.dat", ios::binary | ios::app);
    Balik b;
    char secim;

    do
    {
        cout << "\nBalik No: ";
        cin >> b.balik_no;

        cout << "Balik Turu: ";
        cin >> b.balik_turu;

        cout << "Kilo: ";
        cin >> b.kilo;

        cout << "Kilo Fiyati: ";
        cin >> b.kilo_fiyat;

        cout << "Avlanma Bolgesi: ";
        cin >> b.avlanma_bolge;

        yaz.write((char*)&b, sizeof(Balik));

        cout << "Devam mi? (e/h): ";
        secim = getche();
        cout << endl;

    } while (secim == 'e' || secim == 'E');

    yaz.close();
}

void BalikListele()
{
    ifstream oku("Balik.dat", ios::binary);
    Balik b;
    int sayac = 0;

    while (oku.read((char*)&b, sizeof(Balik)))
    {
        cout << "\n--------------------";
        cout << "\nBalik No: " << b.balik_no;
        cout << "\nTur: " << b.balik_turu;
        cout << "\nKilo: " << b.kilo;
        cout << "\nKilo Fiyati: " << b.kilo_fiyat;
        cout << "\nBolge: " << b.avlanma_bolge;
        sayac++;
    }

    if (sayac == 0)
        cout << "\nKayit yok.";

    oku.close();
}

void BalikAra()
{
    ifstream oku("Balik.dat", ios::binary);
    Balik b;
    char aranan[10];
    bool bulundu = false;

    cout << "Aranan Balik No: ";
    cin >> aranan;

    while (oku.read((char*)&b, sizeof(Balik)))
    {
        if (strcmp(b.balik_no, aranan) == 0)
        {
            cout << "\nTur: " << b.balik_turu;
            cout << "\nKilo: " << b.kilo;
            cout << "\nKilo Fiyati: " << b.kilo_fiyat;
            cout << "\nBolge: " << b.avlanma_bolge;
            bulundu = true;
            break;
        }
    }

    if (!bulundu)
        cout << "\nKayit bulunamadi.";

    oku.close();
}

void BalikSil()
{
    ifstream oku("Balik.dat", ios::binary);
    ofstream yaz("Temp.dat", ios::binary);
    Balik b;
    char silinecek[10];
    bool silindi = false;

    cout << "Silinecek Balik No: ";
    cin >> silinecek;

    while (oku.read((char*)&b, sizeof(Balik)))
    {
        if (strcmp(b.balik_no, silinecek) != 0)
        {
            yaz.write((char*)&b, sizeof(Balik));
        }
        else
        {
            silindi = true;
        }
    }
    oku.close();
    yaz.close();

    remove("Balik.dat");
    rename("Temp.dat", "Balik.dat");

    if (silindi)
        cout << "\nKayit silindi.";
    else
        cout << "\nKayit bulunamadi.";
}

void BalikDuzenle()
{
    ifstream oku("Balik.dat", ios::binary);
    ofstream yaz("Temp.dat", ios::binary);
    Balik b;
    char aranacak[10];
    bool bulundu = false;
    cout << "Duzenlenecek Balik No: ";
    cin >> aranacak;
    while (oku.read((char*)&b, sizeof(Balik)))
    {
        if (strcmp(b.balik_no, aranacak) == 0)
        {
            cout << "\nYeni Tur: ";
            cin >> b.balik_turu;
            cout << "Yeni Kilo: ";
            cin >> b.kilo;
            cout << "Yeni Kilo Fiyati: ";
            cin >> b.kilo_fiyat;
            cout << "Yeni Bolge: ";
            cin >> b.avlanma_bolge;
            bulundu = true;
        }
        yaz.write((char*)&b, sizeof(Balik));
    }

    oku.close();
    yaz.close();

    remove("Balik.dat");
    rename("Temp.dat", "Balik.dat");

    if (bulundu)
        cout << "\nKayit duzenlendi.";
    else
        cout << "\nKayit bulunamadi.";
}
