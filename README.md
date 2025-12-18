<h1>zadanie_1</h1>

### 1) Zacznijmy od zdefiniowania <code>Resource Quota</code> w każdym <code>namespace</code>:

<code>frontend-quota:</code>

<img width="1467" height="75" alt="image" src="https://github.com/user-attachments/assets/649ac121-5016-4c5c-8f9b-6a554814fb7a" />


<code>backend-quota:</code>

<img width="1463" height="80" alt="image" src="https://github.com/user-attachments/assets/e2d630b4-8831-44de-ad5b-3e1822f6bd6a" />


### 2) Następnie wypełnijmy Klaster wszystkimi niezbędnymi zasobami (<code>Deployment</code>, <code>Service</code>, <code>NetPolicy</code>). I zróbmy to tak, aby wszystkie elementy Klastra zostały wdrożone na odpowiednich węzłach (w tym celu wykorzystałem <code>nodeAffinity</code>):



<b>Node Labels:</b>

<img width="1447" height="443" alt="image" src="https://github.com/user-attachments/assets/92aa53c0-9977-4cfe-911d-34edc6b29d73" />



<b>Wszystkie pody są na swoich miejscah:</b>

<img width="1575" height="259" alt="image" src="https://github.com/user-attachments/assets/7d2c66ea-c627-4dbe-bb65-3e190795fa57" />



<b>Serwisy:</b>

<img width="1568" height="178" alt="image" src="https://github.com/user-attachments/assets/4c40c8de-2b6e-4711-a3fe-c26a60826edc" />


### Polityka sieciowa została dobrana w taki sposób, żeby zapewnić wymagane zadaniem zachowanie:

### <b><code>frontend-netpol.yml</code></b>

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1d281b0a-236d-4109-9a0e-a9d9bb02a6b0" />

### <b><code>backend-netpol.yml</code></b>

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0074431a-f06c-4fd6-9ec3-916e3b708a22" />


#### Test <code>NetPolicy</code> z Podów <code>frontend</code>:

<img width="1568" height="652" alt="image" src="https://github.com/user-attachments/assets/046a05b3-f66c-4d78-abf2-98bfdde06a57" />


#### Test <code>NetPolicy</code> z Podów <code>backend</code>:

<img width="1568" height="700" alt="image" src="https://github.com/user-attachments/assets/3b9ba151-c0af-49ec-a03f-119974d2abdb" />


#### <code>HPA</code>

<img width="1554" height="84" alt="image" src="https://github.com/user-attachments/assets/711bc5c7-ee39-4704-bb4a-d93a5238f329" />



## Test Poprawności Konfiguracji Deployment-u <code>frontend</code>

Żeby udowodnić, że konfiguracja Deployment-u <code>frontend</code> dziła poprawnie (może się automatycznie przeskalować), przeprowadziłem prosty test. Polegający na utworzeniu nowego <code>Poda</code> w nowym oknie terminalu, w namespacie <code>default</code> o takiej zawartości: <code>k run -it load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while true; do wget -q -O- http://frontend.frontend.svc.cluster.local; done"</code>, generalnym celem którego jest ciągłe wysłanie zapytań do servis-u <code>frontend</code>, i żeby dziłał poprawnie, zwracam się bezpośrednio do FQDN (Full Quallified Domain Name) servis-u <code>frontend</code>, aby ten pod nie pomylił się z nazwami lokalnymi namespas-u <code>frontend</code>. Rezultatem dziłania jest automatyczne podniesienie ilości podów Deployment-u <code>frontend</code> poprzez <code>HPA</code>:

 Tworzymy obciążenie:
 
 <img width="1561" height="651" alt="image" src="https://github.com/user-attachments/assets/dbc61541-5821-4c28-9aec-3a8964baf27d" />


Widzimy zmiane stanu wykorzystywanych zasobów:

<img width="1562" height="257" alt="image" src="https://github.com/user-attachments/assets/edb3fcc6-bb45-4e22-8c35-231c724775a9" />

## Zadanie Nieobowiązkowe

1) <b>Czy możliwe jest dokonanie aktualizacji aplikacji frontend (np. wersji obrazu
kontenera) gdy aplikacja jest pod kontrolą opracowanego autoskalera HPA ? Proszę do
odpowiedzi (TAK lub NIE) dodać link do fragmentu dokumentacji, w którym jest
rozstrzygnięta ta kweska.</b>

<b>Odpowiedź:</b> Tak, to jest możliwe.

<b>Link do dokumentacji:</b> <a>https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/#autoscaling-during-rolling-update</a>

Można to udowodnić w bardzo prosty sbosób. Zmienimy wersję obrazu <code>nginx</code> na <code>nginx:1.17</code> i zobaczmy co się będzie dziłać.

Z powodu tego, że już to robiłem, mogę się powrócić do pewnej wersji rewizji, tak i zrobiłem:

<img width="1920" height="145" alt="image" src="https://github.com/user-attachments/assets/ab29be04-bba2-4f52-ab65-b63a19616c2b" />
<img width="1921" height="657" alt="image" src="https://github.com/user-attachments/assets/9512e561-715b-4037-b355-388f60f61961" />

Jak widzimy na zrzucie ekranu aktualizacja odbyła się sukcesem, tylko że przez jakiś czas prametry docelowe objektu <code>hpa</code> były tymczasowo niedostępne.

Ale za chwileczkę stan <code>hpa</code> wróci do normalnego:
<img width="1920" height="836" alt="image" src="https://github.com/user-attachments/assets/90471e41-e9c3-4830-825f-7afa9b34c2b6" />

2) <b>Jeśli odpowiedź na poprzednie pytanie jest pozytywna to proszę podać przykładowe
parametry strategii rollingUpdate, które zagwarantują, że: a) Podczas aktualizacji zawsze będa aktywne 2 pod-y realizujące działanie aplikacji
frontend oraz
b) Nie zostaną przekroczone parametry wcześniej zdefiniowanych ograniczeń
zdefiniowanych dla przestrzeni nazw frontend. (ilość Pod-ów, CPU lub RAM). </b>

Żeby osiągnąć takiego zachowania musimy dodać pole <code>strategy</code> wraz z parametrami aktualizacji <code>maxSurge: 0</code> i <code>maxUnavailable: 1</code>. <b>Czemu tak?</b> <code>maxSurge: 0</code> zapewni, że nawet w sytuacji maksymalnej liczby dostępnych podów (=10), liczba przekraczająca podów zawsze będzie równa zero i nie przekroczy limitu. <code>maxUnavailable: 1</code> zapewni, że nawet przy minimalnej liczbie podów (=3), niedostępnych zawsze będzie 1, co znaczy, że zawsze będą dostępne minimalnie 2 pody.

<img width="1917" height="1080" alt="image" src="https://github.com/user-attachments/assets/9fd996d9-9dbf-4ff1-be07-8fa0f23a846a" />

