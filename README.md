# zadanie_1

## 1) Zacznijmy od zdefiniowania <code>Resource Quota</code> w każdym <code>namespace</code>:

<code>frontend-quota:</code>

<img width="1467" height="75" alt="image" src="https://github.com/user-attachments/assets/649ac121-5016-4c5c-8f9b-6a554814fb7a" />

<code>backend-quota:</code>

<img width="1463" height="80" alt="image" src="https://github.com/user-attachments/assets/e2d630b4-8831-44de-ad5b-3e1822f6bd6a" />


## 2) Następnie wypełnijmy Klaster wszystkimi niezbędnymi zasobami (<code>Deployment</code>, <code>Service</code>, <code>NetPolicy</code>). I zróbmy to tak, aby wszystkie elementy Klastra zostały wdrożone na odpowiednich węzłach (w tym celu wykorzystałem <code>nodeSelector</code>):

<b>Labels:</b>

