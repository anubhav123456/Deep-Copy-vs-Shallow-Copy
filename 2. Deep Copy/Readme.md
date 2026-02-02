

---

# 📘 Definition — Deep Copy in Java

**In Java, Deep Copy (or Deep Cloning) refers to the process of creating a completely independent copy of an object, including all the objects referenced within it. This ensures that any changes made to the cloned object do not affect the original object and vice versa.**

**In deep copy:**

* **Primitive fields** → copied by value
* **Non-primitive fields (objects, arrays, collections)** → new objects are created instead of sharing references

---

# ✅ Complete Code (Deep Copy Example)

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

    protected Object clone() throws CloneNotSupportedException
    {
        Boyfriend bf = (Boyfriend) super.clone();  // shallow copy first
        bf.setGirlfriend(new Girlfriend(this.girlfriend.getName())); // deep copy part
        return bf;
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
Girlfriend{name='Ana de Armas'}
Boyfriend{name='Luke', girlfriend=Ana de Armas}
Boyfriend{name='Max', girlfriend=Kate Winslet}
```

---

# 😂 Now Samajh Story Style

### 🎬 Scene 1 — Original Setup

```
Luke → Ana de Armas
```

Memory:

```
Luke ───► GF("Ana")
```

---

### 🤖 Scene 2 — Deep Clone

```java
Boyfriend bf = (Boyfriend) super.clone();
bf.setGirlfriend(new Girlfriend(this.girlfriend.getName()));
```

Yahan kya hua?

1. Boyfriend copy hua ✔
2. **Girlfriend ka bhi NEW object ban gaya** ✔

Memory:

```
Luke ───► GF("Ana")
Max  ───► GF("Ana")  (DIFFERENT OBJECT)
```

Naam same, object alag 😎

---

### 💣 Scene 3 — Max does update

```
max.getGirlfriend().setName("Kate Winslet");
```

Ab kya hoga?

Sirf Max wali girlfriend change hogi
Luke wali unaffected 😌

---

# 🧠 Technical Truth

| Field Type        | Behavior           |
| ----------------- | ------------------ |
| String name       | Copied by value    |
| Girlfriend object | NEW object created |

---

# 📦 Visual Diagram

Before change:

```
Luke ──► GF1("Ana")
Max  ──► GF2("Ana")
```

After change:

```
Luke ──► GF1("Ana")
Max  ──► GF2("Kate")
```

---

# 🏆 Interview One-Liner

> This example demonstrates deep copy because the cloned Boyfriend object contains a new Girlfriend object. Changes to the nested object in the clone do not affect the original.

---
