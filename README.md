# be-dev-angular-bad-practices
mistakes made  by backend dev writing angular/frontend code


## Serialize/Deseralize

### Bad

```typescript
export default class Product implements Serializable<Product> {
  prices: Array<Prices> = new Array<Prices>();
  name: string;
  description: string
  
  deserialize(input: any): Product {
    input.prices.forEach(price => this.prices.push(new Price().deserialize(price)));
    this.name = input.name
    this.description = input.description
    
    return this;
  }
  
  serialize(): Object {
    return {
      prices: this.prices.map(price => price.serialize),
      name: this.name,
      description: this.description
    }
  } 
}

```



### good

```typescript
interface Product {
  prices: Prices[];
  name: string;
  description: string
}
```

## Op vs functional
### Bad

```typescript
export default class Product implements Serializable<Product> {
  prices: Array<Prices> = new Array<Prices>();
  name: string;
  description: string
  constructor(props: Object) {
    Object.assign(this, props);
  }
  
  public getSpecialPrice(): Price | undefined {
    return this.prices.find((price) => price.type === 'special')
  }
}
```



### good

```typescript
getSpecialPrice(prices: Price[]): Price | undefined {
   return prices.find((price) => price.type === 'special');
}
```

Classes are not bad, but it should be used when it makes sense.

## Fat arrow is overuse

### Bad
```javascript
export const getSpecialPrice = (prices: Price[]): Price | undefined  => {
   return prices.find((price) => price.type === 'special');
}
```


### good
```javascript
export function getSpecialPrice(prices: Price[]): Price | undefined {
   return prices.find((price) => price.type === 'special');
}
```

## Avoid subscribe, use  async


## Function params objects are passed by pointer
### bad
```javascript
export function getPoroductWithoutSpecialPrice(product: Product): Product {
    const index = product.prices.findIndex((price) => price.type === 'special');
    if (index > -1) {
      poduct.prices.splice(index, 1)
    }
    return product;
}


const changedProduct = getPoroductWithoutSpecialPrice(initialProdct);

console.log('same prices?', changedProduct.length === initialProdct.length); // -> true

```
this becouse any object sent as a param to a function its passed by pointer, changing any value of the keys will change the initial object as well


### good

```javascript
export function getPoroductWithoutSpecialPrice(product: Product): Product {
    return {
      ...product,
      prices: product.prices.filter((price) => price.type === 'special')
    };   
}

const changedProduct = getPoroductWithoutSpecialPrice(initialProdct);

console.log('same prices?', changedProduct.length === initialProdct.length); // -> false

```
this works becouse the good getPoroductWithoutSpecialPrice generates a new object,
this is expecially important when you use Redux/NgRx as it will only consider a change if the object's reference changes



## NgRx reducers are tricky





## Rxjs switchmap flatmap concat, merge, zip

```
function createObserver(id: number): EventEmitter {

}
```





