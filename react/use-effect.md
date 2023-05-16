# useEffect

```
useEffect( () => console.log("mount"), [] );
useEffect( () => console.log("data1 update"), [ data1 ] );
useEffect( () => console.log("any update") );
useEffect( () => () => console.log("data1 update or unmount"), [ data1 ] );
useEffect( () => () => console.log("unmount"), [] );
```

