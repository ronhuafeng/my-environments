1. `document.getElementById("redeem_reward_form").submit();` 失败的原因是这个表单的 `input` 元素名称就是 `submit`，需要重命名。

  ```javascript
  var field = document.getElementById("chicken");
  field.id = "horse";  // using element properties
  field.setAttribute("name", "horse");  // using .setAttribute() method
  document.getElementById("chicken").setAttribte('name', 'horse');
  document.getElementById("redeem_reward_form").submit.setAttribute('id', 'submit_code')
  ```
  好吧这些都不太管用，应该是 form 之前的 submit 属性已经被设置了，改了名字也不能绑定 submit 方法了。:|
  这个管用：
  ```javascript
  document.getElementById("redeem_reward_form").submit.click()
  ```
2. Post passcode
  ```javascript
  var codes = ['DEADDROP7DT73AM6'];   //Here put the number of times you want to auto submit
  var count = 0;
  (function submitPasscode(){
    if(count >= codes.length) return;
    document.getElementById("redeem_reward_form").passcode.value = codes[count];
    document.getElementById("redeem_reward_form").submit.click();
    count++;
    
    setTimeout(submitPasscode, 3000 + Math.floor((Math.random() * 500) + 1););   //Each second
  })(); 
  ```
  Add a input area:
  ```javascript
  function susubmitPasscodes()
  {
    var codes = document.getElementById('passcodesArea').value.replace( /\n/g, " " ).split(" ");
    var count = 0;
    console.log('Submit code count: ' + codes.length + '\n');
    function submitPasscode(){
      if(count >= codes.length) 
      {
        count=0;
        alert('All passcodes are processed.');
        return;
      }
      document.getElementById("redeem_reward_form").passcode.value = codes[count];
      console.log('Submit code: ' + codes[count] + '\n');
      document.getElementById("redeem_reward_form").submit.click();
      count++;
      
      setTimeout(submitPasscode, 25000 + Math.floor((Math.random() * 25000) + 1)); 
    }; 
    submitPasscode();
  };

  var passcodeBox = document.getElementById('header_passcode_box');

  
  var passcodesArea = document.createElement('textarea');
  passcodesArea.id = 'passcodesArea';
  passcodeBox.appendChild(passcodesArea);
  
  var batchSubmit = document.createElement("button");
  //<button onclick="myFunction()">Click me</button>
  batchSubmit.type = 'button';
  batchSubmit.name = 'batch-submit';
  batchSubmit.value = 'Submit';
  batchSubmit.onclick = susubmitPasscodes;
  batchSubmit.textContent = 'batchSubmit'
  passcodeBox.appendChild(batchSubmit);
  ```
