<h1>zadanie_1</h1>

## 1) Zacznijmy od zdefiniowania <code>Resource Quota</code> w każdym <code>namespace</code>:

<code>frontend-quota:</code>

<img width="1467" height="75" alt="image" src="https://github.com/user-attachments/assets/649ac121-5016-4c5c-8f9b-6a554814fb7a" />

<code>backend-quota:</code>

<img width="1463" height="80" alt="image" src="https://github.com/user-attachments/assets/e2d630b4-8831-44de-ad5b-3e1822f6bd6a" />


## 2) Następnie wypełnijmy Klaster wszystkimi niezbędnymi zasobami (<code>Deployment</code>, <code>Service</code>, <code>NetPolicy</code>). I zróbmy to tak, aby wszystkie elementy Klastra zostały wdrożone na odpowiednich węzłach (w tym celu wykorzystałem <code>nodeSelector</code>):


<b>Node Labels:</b>

<img width="1447" height="443" alt="image" src="https://github.com/user-attachments/assets/92aa53c0-9977-4cfe-911d-34edc6b29d73" />


<b>Wszystkie pody są na swoich miejscah:</b>

<img width="1575" height="259" alt="image" src="https://github.com/user-attachments/assets/7d2c66ea-c627-4dbe-bb65-3e190795fa57" />


<b>Serwisy:</b>

<img width="1568" height="178" alt="image" src="https://github.com/user-attachments/assets/4c40c8de-2b6e-4711-a3fe-c26a60826edc" />


### Polityka sieciowa została dobrana w taki sposób żeby zapewnić wymagane zadaniem zachowanie:

<b><code>frontend-netpol.yml</code></b>

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1d281b0a-236d-4109-9a0e-a9d9bb02a6b0" />

<b><code>backend-netpol.yml</code></b>
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0074431a-f06c-4fd6-9ec3-916e3b708a22" />


Test <code>NetPolicy</code> z Podów <code>frontend</code>:

<img width="1568" height="652" alt="image" src="https://github.com/user-attachments/assets/046a05b3-f66c-4d78-abf2-98bfdde06a57" />


Test <code>NetPolicy</code> z Podów <code>backend</code>:

<img width="1568" height="700" alt="image" src="https://github.com/user-attachments/assets/3b9ba151-c0af-49ec-a03f-119974d2abdb" />


