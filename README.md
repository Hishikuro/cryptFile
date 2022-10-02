> # cryptFile

# Description :

Ce programme permet de **chiffré / déchiffré un fichier texte** avec **un mot de passe** 

(rq : un exercice d'entrainement )

# Info :

## bin :  executable

- textcode.exe  (windows)
- textcode  (linux)

## src : source code  

- textcode.cpp 

  ```c++
  #include <iostream>
  #include <string>
  #include <chrono>
  #include <fstream>
  
  using namespace std;
  bool existFile(string url){
      //! WARNING Trop generique  
      fstream file;
      file.open(url.c_str());
      if(file.is_open() == false) {
         file.close();
         return false;
     } else {
        return true;
     }
  }
  string readFileData(string url){
  ifstream f(url);
  string data;
  if(f){
      string line;
      while(getline(f,line)){
          cout << line << endl;
          data += line;
      }
  }
  else{
      cout << "Erreur il  est impossible de lire le fichier " << endl;
  }
  
  return data;
  }
  
  void writeFile(string url , string text){
      
      ofstream mflux(url.c_str());
      if(mflux){
          mflux << text;
      }
      else{
           cout << "Erreur il  est impossible de ecrire dans  le fichier " << endl;
      }
  }
  
  void separate(string caracter , int nb){
         for (int i = 0; i <nb;i++){
              cout << caracter ;
          }
          cout << endl;
  }
  string crpt(string text , string symbole , string code){
  
      int textlen = text.length();
      int textcode = code.length();
      int nbCode = textlen / textcode; 
  
      string codeabsolut;
      for (int i = 0; i <nbCode; i++){
          codeabsolut+= code;
  
      }
     
      int nbCodeReste = textlen % textcode; 
      for (int i = 0; i < nbCodeReste; i++){
          codeabsolut+= code[i];
      }
      
  
      string newtext;
      if (symbole == "+")
      {
          for (int i = 0; i < text.length(); i++){
          newtext+= text[i] + codeabsolut[i];
           }
      }
      else if (symbole == "-")
      {
          for (int i = 0; i < text.length(); i++){
          newtext+= text[i] - codeabsolut[i];
           }
      }
      
      return (newtext);
      
  }
  void decrypt(string url,string code){
  
      writeFile(url ,crpt(readFileData(url),"-",code));
  }
  
  void crypt(string url,string code){
      writeFile(url ,crpt(readFileData(url),"+",code));
  
  }
   int main(int argc, char const *argv[])
   {
      cout << ":INFO:"<<endl;
  
          separate("#" , 20 );
          
         
          string url(argv[1]); 
              string type(argv[2]);
              string code(argv[3]);
             
  
              if (type != "-D" && type !="-d" && type !="d" && type !="D" && type != "-C" && type !="-c" && type !="c" && type !="C" ){
      
                  separate("-" , 10);
                  cout << "Erreur : - Mauvais mode -"<<endl;
                  cout <<argv[0]<<" file"<<" -mode"<<" code"<<endl;
                  cout <<"mode:"<<endl<<"-D : decrypt "<<endl<<"-C : crypt "<<endl;
                  separate("-" , 10);
                  return 0;
              
              }
              if(code.length() == 0){
                  separate("-" , 10);
                  cout << "Erreur : - Code non conforme -"<<endl;
                  cout << "Code Non saisie"<<endl;
                  separate("-" , 10);
                  return 0;
  
              }
              if(existFile(url) == false) {
                  separate("-" , 10);
                  cout << "Erreur : - Url non comforme -"<<endl;
                  cout << "Fichier Non Trouver"<<endl;
                  separate("-" , 10);
                  return 0;
  
              }
              
              cout<<"URL :" << url<<endl;
              cout<<"Type :"<<type<<endl;
              cout <<"Code :"<<code<<endl;
  
              auto start = chrono::steady_clock::now();
              if(type == "-D" || type =="-d" || type =="d" || type =="D"){
                   decrypt(url,code);
  
              }
              else if (type == "-C" || type =="-c" || type =="c" || type =="C")
              {
                  crypt(url,code);
              }
             
              auto end = chrono::steady_clock::now();
              
              cout<<"FIN "<<"en "<<chrono::duration_cast<chrono::nanoseconds>(end-start).count()<<" Nanoseconde"<<endl;
  
          
  
          separate("#" , 20 );
  
      
  
      
  
      return 0;
  
   }
   
  ```

  

## file : Exemple de fichier pour teste

### text2.txt
Type  actuelle :-C
Code :78456
av.modification: 

```
VA DORMIR !
```

apr.modification : 

```
yTy…‰…}‡VX
```



### text3.txt 

Type actuelle  :-C
Code :fjirei7
av.modification: 

```
VA DORMIR !
```

apr.modification : 

```
¼«‰¶´»„¯¼‰“
```

### text.txt (original)

Type actuelle  :-D et non chiffrée et déchiffré

```
VA DORMIR !
```



# Utilisation:

- ouvrir "**l'inviter de commande**" 
- taper la commande "**cd  'emplacement du fichier'** "
- commande suivant :  
    - windows : 
        **textcode.exe file -mode code**
    - linux : 
        **./textcode file -mode code** 



