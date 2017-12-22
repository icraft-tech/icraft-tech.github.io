---
layout: post
title: "AngularMaterialDatePickerの表示フォーマットで詰まった話"
date: 2017-12-19 00:00:00 -0700
tags:
- Angular
- AngularMaterial
- TypeScript
---

仕事で`Angular Material Datepicker` を使用する機会があり、使用してみたところ  
`chrome` や `safari` だと `2017/12/16` のように `/`区切りで表示されるのですが  
`IE11` だけ `2017年12月16日` のように年月日で表示される事がわかりました  
ソースコードを読んでみたところ、表示の際に `NaitiveDataAdapter` クラスの `format` 関数を呼び出している事が判明したため   
`NaitiveDataAdapter` を継承した `MyDateAdapter` を作成し、 `format` 関数をオーバライドする事にしました
``` ts
export class MyDateAdapter extends NativeDateAdapter {
  constructor() {
    super('ja'); // デフォルトだとアメリカ時間になっているので日本にする
    this.setLocale('ja');
  }
  // format関数を上書き
  format(date: Date, displayFormat: Object): string {
    const day = date.getDate();
    const month = date.getMonth() + 1;
    const year = date.getFullYear();
    return year + '/' + ('00' + month).slice(2) + '/' + ('00' + day).slice(2);
  }
}
```

その後、`format` 関数を上書きした `MyDateAdapter` を `DateAdapter` にインジェクションする事により`IE11`でも`/`区切りで表示されるようになりました  
```ts
    { provide: DateAdapter, useClass: MyDateAdapter }, // DateAdapterに対してMyDateAdapterのインスタンスを使用するようにする
```
