

---

# 📘 Definition 

**In Java, Shallow copy means creating a clone of an object, where the object itself is copied but the nested objects are not copied. Instead of duplicating the nested objects, a shallow copy only copies the reference to those nested objects. This means that both the original object and the copied object share the same references to the nested objects. The same change is reflected in the copied object.**

---

# ✅ Complete Code 

```java
class Girlfriend
{
    private String name;

    public Girlfriend(String name)
    {
        this.name = name;
    }

    public String getName()
    {
        return name;
    }

    public void setName(String name)
    {
        this.name = name;
    }

    @Override
    public String toString()
    {
        return "Girlfriend{" +
                "name='" + name + '\'' +
                '}';
    }
}

class Boyfriend implements Cloneable
{
    private String name;
    private Girlfriend girlfriend;

    public Boyfriend(String name, Girlfriend girlfriend)
    {
        this.name = name;
        this.girlfriend = girlfriend;
    }

    public String getName()
    {
        return name;
    }

    public void setName(String name)
    {
        this.name = name;
    }

    public Girlfriend getGirlfriend()
    {
        return girlfriend;
    }

    public void setGirlfriend(Girlfriend girlfriend)
    {
        this.girlfriend = girlfriend;
    }

    protected Object clone() throws CloneNotSupportedException
    {
        return super.clone(); // SHALLOW COPY
    }

    @Override
    public String toString()
    {
        return "Boyfriend{" +
                "name='" + name + '\'' +
                ", girlfriend=" + girlfriend.getName() +
                '}';
    }
}

public class Main
{
    public static void main(String[] args) throws CloneNotSupportedException
    {
        Girlfriend gf = new Girlfriend("Ana de Armas");
        Boyfriend luke = new Boyfriend("Luke", gf);
        Boyfriend max = (Boyfriend) luke.clone();

        System.out.println(max == luke); // false

        max.setName("Max");
        max.getGirlfriend().setName("Kate Winslet");

        System.out.println(gf);
        System.out.println(luke);
        System.out.println(max);
    }
}
```

---

# 🖨 OUTPUT

```
false
Girlfriend{name='Kate Winslet'}
Boyfriend{name='Luke', girlfriend=Kate Winslet}
Boyfriend{name='Max', girlfriend=Kate Winslet}
```

---

# 😂 Now Explanation in “Relationship Disaster” Style

### 🎬 Scene 1 — Original Setup

```
Girlfriend gf = "Ana de Armas"
Boyfriend luke → gf
```

Memory:

```
Luke ───► Girlfriend("Ana de Armas")
```

Sab shaanti 😌

---

### 🤖 Scene 2 — Clone Machine

```
Boyfriend max = (Boyfriend) luke.clone();
```

Ye line karta kya hai?

* Boyfriend object copy ho gaya
* BUT girlfriend object copy nahi hua
* Reference SAME

Memory becomes:

```
Luke ───► Girlfriend("Ana")
Max  ───► Girlfriend("Ana")
```

Do bande. Ek girlfriend.
**Ye Java ka Bigg Boss house hai** 😂

---

### 🔍 Proof

```
System.out.println(max == luke); // false
```

Dono alag objects ✔

But internally:

```
max.getGirlfriend() == luke.getGirlfriend()  → true
```

Same girlfriend object 😭

---

### 💣 Scene 3 — Max Creates Drama

```
max.setName("Max"); // safe
max.getGirlfriend().setName("Kate Winslet"); // chaos
```

Max ne girlfriend ka naam change kiya…

Par girlfriend shared thi…

Toh Luke ka kya haal?

**Without interview, without notice — girlfriend update ho gayi** 💀

---

### 🖨 Output Meaning

| Print                             | Meaning                    |
| --------------------------------- | -------------------------- |
| `Girlfriend{name='Kate Winslet'}` | Original gf object updated |
| `Luke → Kate Winslet`             | Luke shocked 😭            |
| `Max → Kate Winslet`              | Max confident 😎           |

---

# 🧠 Technical Breakdown

Your clone method:

```java
return super.clone();
```

Ye karta hai:

✔ New Boyfriend object
✔ Same Girlfriend reference
❌ No deep copy

---

# 📦 Visual Diagram

Before change:

```
Luke ──► GF("Ana")
Max  ──► GF("Ana")
```

After change:

```
Luke ──► GF("Kate")
Max  ──► GF("Kate")
```

---

# 🏆 Interview One-Liner

> This code demonstrates a shallow copy because the Boyfriend object is cloned, but both the original and cloned objects share the same Girlfriend reference. Any change to the nested object is reflected in both.

---
