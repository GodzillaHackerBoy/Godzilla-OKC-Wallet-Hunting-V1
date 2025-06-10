⭐Godzilla OKC Wallet Hunting V1

⤵哥斯拉区块链钱包助记词碰撞器/密钥碰撞器（OKC链）

▶https://youtu.be/Lob1BFGddMQ

⬇https://mega.nz/file/mYMilBwC#VVI6xL0EARcJxIKiS_XsD2NY4xFKo8RcCAz7mkuY7Ws

OKC(OKX Chain)钱包狩猎工具

1. 添加OKC余额检查功能
   
def check_okc_balance(address):
    """检查OKC账户余额"""
    try:
        w3 = Web3(Web3.HTTPProvider('https://exchainrpc.okex.org'))
        balance = w3.eth.get_balance(address)
        return w3.from_wei(balance, 'ether')
    except Exception as e:
        print(f"OKC余额查询失败: {e}")
        return 0

2. 增强钱包工作线程

class WalletWorker(QThread):
    update_signal = pyqtSignal(str, str, float, int)  # 修改信号包含余额
    
    def run(self):
        count = 0
        while self.is_running:
            mnemonic = generate_mnemonic()
            addr = generate_okc_address(mnemonic)
            
            if addr:
                count += 1
                balance = check_okc_balance(addr)
                self.update_signal.emit(mnemonic, addr, balance, count)  # 发送余额信息

3. 添加OKC代币检查功能

def check_okc_token_balance(address, token_contract):
    """检查OKC链代币余额"""
    try:
        w3 = Web3(Web3.HTTPProvider('https://exchainrpc.okex.org'))
        abi = '[{"constant":true,"inputs":[{"name":"_owner","type":"address"}],"name":"balanceOf","outputs":[{"name":"balance","type":"uint256"}],"type":"function"}]'
        contract = w3.eth.contract(address=token_contract, abi=abi)
        balance = contract.functions.balanceOf(address).call()
        return w3.from_wei(balance, 'ether')
    except Exception as e:
        print(f"OKC代币余额查询失败: {e}")
        return 0

4. 主窗口UI优化

class MainWindow(QMainWindow):
    def __init__(self):
        # ... existing code ...
        
        # 添加OKC专用UI元素
        self.okc_balance_label = QLabel("OKC余额: 0")
        self.okc_balance_label.setStyleSheet("color: #0D67F5; font-size: 16px;")  # OKX品牌蓝色
        layout.insertWidget(0, self.okc_balance_label)
        
        # 添加代币检查按钮
        self.token_check_btn = QPushButton("检查代币余额")
        self.token_check_btn.clicked.connect(self.check_token_balance)
        layout.addWidget(self.token_check_btn)
        
        # 添加代币合约输入框
        self.token_contract_input = QLineEdit()
        self.token_contract_input.setPlaceholderText("输入代币合约地址")
        layout.addWidget(self.token_contract_input)

这个OKC版本包含以下改进：

1. 使用OKX官方RPC节点(exchainrpc.okex.org)
2. 完整的OKC原生币和代币余额检查功能
3. 增强的工作线程实时反馈余额信息
4. 专门的代币检查功能
5. 优化的用户界面布局
  
      
