
# Java Basic

## Reflection
```java
// cat's name is private and final
Cat cat1 = new Cat("guang dang", 6);
Field[] fields = cat1.getClass().getDeclaredFields();

// fields gets all attribute int he class

// Yet by reflection, you can modify cat's name
if (field.getName().equals("catName")) {
    field.setAccessible(true);
    field.set(cat1, "Guang Dang");
}

Method[] catMethods = cat1.getClass().getDeclaredMethods();

if (method.getName().equals("meow")) {
    method.invoke(cat1);
    // >>> "Meow Meow"
}
```

