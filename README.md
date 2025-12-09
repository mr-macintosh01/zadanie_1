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



