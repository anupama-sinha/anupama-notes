### Types of Mapping 
#### @OneToOne - One Product is owned by one member
* It's always a best practice to put mapping in PK class who owns and has FK mapping of owned entity.
```java
@OneToOne
private Members members;
```
#### @OneToMany(One Member can own multiple Products)
* This mapping loads lazily by default hence leading to optimizing the performance. How? 
Say we have 50 members, with 100000 products. Each time, we need 1 member, entire product will be loaded for this operation. So, better load products only when required. Thats's lazy loading.
* Optional relationship type

#### @ManyToOne(Not used here but eg. Many Products owned by One Member)
* Eager by default. So, don't go for this unless required.
* @ManyToOne(fetch = FetchType.LAZY) - This again makes it lazy. But with same example as above, would you want to load entire 100000 products and work in the reverse way? No? Right, choice is yours.
* This relationship type is also optional by default but can be made mandatory - @ManyToOne(optional = false)
* If relationship type is mandatory, we can't save Member without a Product. 

#### @ManyToMany(One member can read multiple books. And one book can be read by multiple members)
* Configurations for optionality, fetch type, cascading effect can be done as in others 

### Common Tips
* referencedColumnName isn't required for PK mapping. For non-PK mapping, it is required
