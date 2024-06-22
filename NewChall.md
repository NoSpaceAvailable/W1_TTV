# Description
- Author: dcthinh
- Type: blackbox

# Recon
- Dựa vào challenge description thì mình thấy rằng khi truy cập vào web, ta có thể thực hiện các phép toán cơ bản ở endpoint *exec* với tham số là *q*.
- Khi thử thực thi các đoạn mã javascript như require hay exec thì web trả về message thông báo đại loại kiểu *Are you hacking?* nên mình khá chắc rằng phía server đang thực thi input người dùng bằng một hàm thực thi như eval()
- Mình tiếp tục fuzz bằng cách dùng keyword *import* được encode thành octal (mục tiêu là import được một module gì đó như là *child_process*). Khi này thì server trả về lỗi *Cannot use import statement outside a module*. Lỗi này xuất hiện do phía server dùng NodeJS làm backend (CommonJS), trong khi import chỉ nên được dùng khi mã JS được viểt ở định dạng ECMAScript.
- Sau một lúc fuzz nữa thì em chắc chắn rằng phía server dùng hàm eval() để thực thi nên em thực hiện exploit bằng *Function* object như sau:
  ```
  Function("return \160\162\157\143\145\163\163\056\155\141\151\156\115\157\144\165\154\145\056\162\145\161\165\151\162\145\050\047\143\150\151\154\144\137\160\162\157\143\145\163\163\047\051\056\145\170\145\143\123\171\156\143\050\047\143\141\164\040\146\154\141\147\056\164\170\164\047\051")();
  ```
- Đoạn octal code thực thi câu lệnh: *process.mainModule.require('child_process').execSync('cat flag.txt')*
- Flag: W1{hehehe}
