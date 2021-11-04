---
sidebar_position: 12
---

# U47-49 表格驗證(簡易版)

## 驗證
1. 目的是在伺服器正式處理資料前，先在前端提前驗證使用者的輸入內容是否符合能處理的格式，並讓後端減少進行判斷的負擔。

## novalidate 
1. 表單元件 form 擁有的屬性之一
2. 用途是關閉瀏覽器對於表格的預設自動驗證功能(包含調整樣式)，並讓瀏覽器不再阻擋沒通過驗證的表單以及渲染對應的錯誤提示。
```
<form novalidate>

</form>
```
3. 若沒設定novalidate的話，瀏覽器預設上會在第一時間阻擋到沒通過驗證的情況並做處理。

## checkValidity()
1. DOM表單物件會有的JS方法。
2. 主要是確定表單(含表單子元件)所截取的資料是否符合預設標準，若符合就回傳true，若不符合就為false
3. 形式為：

```
const form = document.querySelector('#form')
form.checkValidity()
```

4. 預設標準為：
 - [HTML標準對於checkValidity的標準](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#the-constraint-validation-api)
 - [Validity的標準](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Constraint_validation)

## 負責驗證的偽類
1. 會根據以下標準所定下的內容來判別對應元件所截取的內容是否正確：
 - [HTML標準對於checkValidity的標準](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#the-constraint-validation-api)
 - [Validity的標準](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Constraint_validation)
2. 根據條件來做驗證的偽類(Pseudo-Class)主要有以下類別：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1635324990/blog/dom/validationPseudoClass_ens4le.png)

3. 舉例：會先找到selector對應的元件是什麼，當如果它是能擷取使用者資料的元件時，便會由系統根據標準來自動判定擷取到的內容是否合法，若不合法的話，就以selector:invalid指定的樣式來渲染selector對應的元件
```
selector:invalid {
  property1: value1,
        .
        .
        .
}
```

## sibling combinator
1. 是 class 用來與其他 class 進行連接/組合的符號(operator)，在這裡會挑同階層的元素。
2. 形式會是如下，會挑與A對應元素X同階層且在對應元素X之後的對應元素Y，而對應元素Y正是B類別所對應的元素
```
A ~ B {
    property1: value1,
        .
        .
        .
}
```

3. 舉例：會直接挑選一個在p元素同階層且在p元素之後的span，也就是第四段的And here is a span.

```
// Inside css file
p ~ span {
  color: red;
}

// Inside html file
<span>This is not red.</span>
<p>Here is a paragraph.</p>
<code>Here is some code.</code>
<span>And here is a span.</span>
```

結果會是

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1635328112/blog/cssTag/siblingCSSExample_kbcwlq.png)




## 手刻驗證功能
1. 關閉瀏覽器對於前端的預設自動驗證功能(包含根據驗證結果來調整樣式的機制)，並讓瀏覽器不再第一時間對於沒通過驗證的表單進行樣式渲染和阻止提交事件的事件處理，由於這部分要自行決定驗證樣式，所以要藉由novalidate來實現
```
<form novalidate>

</form>
```
2. 關閉瀏覽器對於提交事件的預設行為，主要停止信號繼續傳遞以及取消掉預設的提交事件處理
```
const form = document.querySelector('#form')
form.addEventListener('submit', function onFormSubmitted(event) {
  if (!form.checkValidity()) {
    // 因為輸入資料是不符合規則，所以事件信號傳遞只會是多餘的，所以得停止事件信號傳遞
    event.stopPropagation()
    // 取消掉瀏覽器預設的行為來讓開發者自行開發這行為的實現
    event.preventDefault()
  }
})
```

3. 設計對於驗證結果的樣式，主要會透過偽類來渲染符合標準的內容和不符合標準的內容，形式會是如下，而selector會對應一個可擷取內容的元件，當內容不合法就跳到invalid這個偽類所指示的樣式內容來渲染對應元件，而當內容合法就跳到valid這個偽類所指示的樣式內容，然而在使用者未進行任何輸入時，對應元件有可能會立刻被判定為非法(invalid)而給予適當的樣式，若預期結果會是要輸入之後或者提交後才給予驗證樣式的話，這樣形式會是錯誤的

```
selector:invalid {
  property1: value1,
        .
        .
        .
}
selector:valid {
  property1: value1,
        .
        .
        .
}
```

4. 承第三點，若預期結果會是要輸入之後或者提交後才給予樣式的話，只需要在負責通知提交的元件上綁定點擊事件讓它去更動對應元件的類別名稱(如下面的第一個方法)或者更動表單本身的類別名稱(如下面的第二個方法)，若採用前者的話，按下提交時每個表單下的子元件都必須添加額外類別，而若採用後者的話，只需要在按下提交時替表單(form)添加額外類別，就能讓當下需要驗證樣式的所有元件都能獲得其樣式。前者比起後者還更多餘，所以一般來說會選擇後者。
```
// first method
selector.className:invalid {
  .
  .
}

// second method
form.className selector:invalid {
  .
  .
  .
}
```

5. 在這裏會採用第四點所提到的後者形式來展現，這裏會先替表單(form)定義額外的類別名稱來幫助系統判別什麼時候這個表單已經被驗證過，另外再透過提交事件或者某個元件的點擊事件來添加這額外的類別名稱，來產生出提交後才給予驗證樣式的效果：
 - 首先先定義表單的額外類別，並附加至其他元件的合法情況下的樣式和不合法情況下的樣式：讓他們能在表單的類別為was-validated，才開始按照合不合法來渲染。
```
form.was-validated selector:invalid {
  property1: value1,
        .
        .
        .
}
form.was-validated selector:valid {
  property1: value1,
        .
        .
        .
}
```

- 設定表單的點擊事件來替表單增加was-validated這樣式名稱來讓系統去執行上面提到的樣式，當使用者去點擊按鈕時，便會讓表單類別有了was-validated這項類別，這時selector的偽類就會開始進行合適的渲染
```
const submitButton = document.querySelector('#submit-button')
submitButton.addEventListener('click', function onSubmitButtonClicked(event) {
  form.classList.add('was-validated')
})
```

6. 除了selector本身對應的輸入元件要顯示驗證樣式以外，還得顯示該元件的獨立驗證訊息來當作該元件的額外驗證資訊，比如告知使用者目前輸入內容是合法的訊息、告知使用者目前輸入內容是非法的訊息，首先會先建立以下兩個區塊，前者區塊代表著合法訊息，後者區塊代表著非法訊息，兩個區塊類別分別為valid-feedback和invalid-feedback，這兩個樣式會在驗證前保持不顯示，直到表單出現提交事件才開始顯示，詳細內容可看第8點。

```
// Inside html file
 <div class="valid-feedback">
  Good!
 </div>
 <div class="invalid-feedback">
  This field is required!
</div>

// Inside css file
.invalid-feedback, .valid-feedback {
  display: none;
  font-size: 0.8em;
}
.invalid-feedback {
  color: red
}
.valid-feedback {
  color: green
}
```
7. 最後再將這些訊息放在他們針對的輸入元件附近來方便管理
```
// Inside html file
<form novalidte>
  <label for="name">姓名</label>
  <input type="text" class="form-control" name="name" id="name" placeholder="你輸入你的姓名" required>
  <div class="valid-feedback">
  Good!
  </div>
  <div class="invalid-feedback">
  This field is required!
  </div>
</form>
```

8. 承第6-7點，為了達到直到做提交才出現合法訊息或者不合法訊息，必須做類似第5點要做的事情，也就是增加一些類別當作條件，在這裡會判斷現在表單類別是否為was-validated這選擇器名以及鄰近是否有control元件(輸入元件)當作條件，而其中initial會以display的預設值，也就是inline，若前面的條件都符合的話，就依照initial來顯示display

```
form.was-validated .form-control:invalid ~ .invalid-feedback,
form.was-validated .form-control:valid ~ .valid-feedback {
  display: initial;   /* 顯示 */
}
```



## Bootstrap 表單
1. Bootstrap會透過form-group這個類別來包裝一個輸入欄位所需要的內容，比如輸入介面元件(control元件)、提示字元(輸入介面元件上的標籤和說明文字)、驗證訊息元件，其形式會是如下，首先會先建立類別form-group的div元件，並在裡頭建立用輸入介面元件(input元件)、提示字元元件(label元件)、驗證訊息元件(後面兩個div元件)。
```
<form>
  <div class=form-group>
      <label for="name">姓名</label>
      <input type="text" class="form-control" id="name" placeholder="請輸入你的全名 ..." required>
      <div class="valid-feedback">OK!</div>
      <div class="invalid-feedback">This field is required.</div>
  </div>
</form>
```
2. 表單下的任意一個能透過互動來操控特定資訊的編輯/讀取/輸入的元件都會用form-control這個類別來標記，尤其是input元件、select元件、textarea元件。

```
<input class="form-control"></input>
<select class="form-control"></select>
<textarea class="form-control"></textarea>
```

3. 舉例：首先用form建立一個表單，在這裡只要添加一個輸入元件來決定姓名，而一個輸入元件通常會綁定一些提示元件標籤和說明字元、輸入元件的驗證訊息元件，所以為了好分類而透過form-group將相關的元件綁定在一塊管理，裡頭會定義提示使用者這是輸入姓名的輸入欄位、輸入欄位、驗證元件，分別由label、input、兩個div元件，label元件負責定義輸入提示，input元件則是定義輸入欄位、兩個div元件負責用Bootstrap內建的valid-feedback和invalid-feedback類別來建立驗證訊息元件並根據驗證結果來呈現訊息。

```
  <div class="contain">
    <div class="row mx-2">
      <div class="col-6">
        <form novalidate action="/test" id="form">
          <div class="form-group">
            <label for="name">姓名</label>
            <input type="text" class="form-control" placeholder="請輸入你的姓名..." id="name" required>
            <div class="valid-feedback">
              Good!!
            </div>
            <div class="invalid-feedback">
              Please choose a username
            </div>
          </div>
          <button class="btn btn-primary" type="submit">送出</button>
        </form>
      </div>
    </div>
  </div>
```

## Bootstrap 表單驗證

1. Bootstrap提供一些helper class來建立表單驗證所需的元件，比如針對使用者輸入來顯示內容對錯的訊息、圖標、樣式。
2. Bootstrap 主要用來驗證的 helper class：
  - was-validated：會套用在表單上，通常用來告訴系統該表單已經在前端驗證過了，必須先讓表單出現這個類別才能讓下面的helper class在驗證時顯示正確的樣式。
  - form-group：主要會套用在表單中一個負責給予輸入的control元件以及該control相關的元件(如使用提示元件、驗證訊息元件)，來讓被包含的元件根據control元件所獲取的內容驗證結果來給予正確的樣式，當主表單的類別為was-validated時，form-group對應元件就會根據使用者輸入內容來呈現樣式。
  ```
  form.was-validated .form-control:invalid {
    border-color: red;
  }

  form.was-validated .form-control:valid {
    border-color: green;
  }
  ```

  - valid-feedback：主要會套用驗證訊息元件上，一開始該元件不會呈現，並會根據主表單是否為was-validated以及鄰近的control元件所獲取的內容是否正確，若為主表單是was-validated且鄰近的control元件所獲取的內容是正確的，就依照預設的display值來顯示。
  ``` 
  .invalid-feedback,
  .valid-feedback {
    display: none;
    font-size: 0.8em;
  }

  form.was-validated .form-control:valid ~ .valid-feedback {
    display: initial;
  }
  ```
  - invalid-feedback：主要會套用驗證訊息元件上，一開始該元件不會呈現，並會根據主表單是否為was-validated以及鄰近的control元件所獲取的內容是否不正確，若為主表單是was-validated且鄰近的control元件所獲取的內容是不正確的，就依照預設的display值來顯示。

  ```
  .invalid-feedback,
  .valid-feedback {
    display: none;
    font-size: 0.8em;
  }

  form.was-validated .form-control:invalid ~ .invalid-feedback {
    display: initial;
  }

  ```
- 完整樣式大致上會以下面為主：

```
input.form-control {
  border: none;
  border-bottom: 1px solid #757575;
  background-color: #f5f5f5;
}

.invalid-feedback,
.valid-feedback {
  display: none;
  font-size: 0.8em;
}

.invalid-feedback {
  color: red;
}

.valid-feedback {
  color: green;
}

form.was-validated .form-control:invalid ~ .invalid-feedback {
  display: initial;
}

form.was-validated .form-control:valid ~ .valid-feedback {
  display: initial;
}

form.was-validated .form-control:invalid {
  border-color: red;
}

form.was-validated .form-control:valid {
  border-color: green;
}
```