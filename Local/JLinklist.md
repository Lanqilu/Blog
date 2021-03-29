```
if(list instanceof RandomAccess) {
    //随机访问
    for (int i = 0; i < list.size(); i++) {
        User user = list.get(i);
        System.out.println(user);
    }
}else{
    //顺序访问
    for (User user : list) {
        System.out.println(user);
    }
}
```