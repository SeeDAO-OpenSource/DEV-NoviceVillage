# 概论

以太坊最常见的用途之一是由一个团队来打造一种可以交易的代币，在某种意义上是他们自己的货币。这些代币通常遵循一个 ERC-20 标准，ERC-20 为以太坊智能合约提供了一套编写规范，此标准使得人们能够以此来开发可以用于所有 ERC-20 代币的工具，以下将对简单的标准源码进行展示和解读。

# ERC-20 源码展示

[ERC-20](https://ide.black/BlackIDE-ObsidianLab/ERC-20) 具体代码如下所示：

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import "./IERC20.sol";
import "./IERC20Metadata.sol";
import "./Context.sol";

/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {ERC20PresetMinterPauser}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.zeppelin.solutions/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * We have followed general OpenZeppelin guidelines: functions revert instead
 * of returning `false` on failure. This behavior is nonetheless conventional
 * and does not conflict with the expectations of ERC20 applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IERC20-approve}.
 */
contract ERC20 is Context, IERC20, IERC20Metadata {
  mapping(address => uint256) private _balances;

  mapping(address => mapping(address => uint256)) private _allowances;

  uint256 private _totalSupply;

  string private _name;
  string private _symbol;

  /**
   * @dev Sets the values for {name} and {symbol}.
   *
   * The default value of {decimals} is 18. To select a different value for
   * {decimals} you should overload it.
   *
   * All two of these values are immutable: they can only be set once during
   * construction.
   */
  constructor(
    string memory name_,
    string memory symbol_,
    uint256 totalSupply
  ) {
    _name = name_;
    _symbol = symbol_;
    _mint(_msgSender(), totalSupply);
  }

  /**
   * @dev Returns the name of the token.
   */
  function name() public view virtual override returns (string memory) {
    return _name;
  }

  /**
   * @dev Returns the symbol of the token, usually a shorter version of the
   * name.
   */
  function symbol() public view virtual override returns (string memory) {
    return _symbol;
  }

  /**
   * @dev Returns the number of decimals used to get its user representation.
   * For example, if `decimals` equals `2`, a balance of `505` tokens should
   * be displayed to a user as `5,05` (`505 / 10 ** 2`).
   *
   * Tokens usually opt for a value of 18, imitating the relationship between
   * Ether and Wei. This is the value {ERC20} uses, unless this function is
   * overridden;
   *
   * NOTE: This information is only used for _display_ purposes: it in
   * no way affects any of the arithmetic of the contract, including
   * {IERC20-balanceOf} and {IERC20-transfer}.
   */
  function decimals() public view virtual override returns (uint8) {
    return 18;
  }

  /**
   * @dev See {IERC20-totalSupply}.
   */
  function totalSupply() public view virtual override returns (uint256) {
    return _totalSupply;
  }

  /**
   * @dev See {IERC20-balanceOf}.
   */
  function balanceOf(address account)
    public
    view
    virtual
    override
    returns (uint256)
  {
    return _balances[account];
  }

  /**
   * @dev See {IERC20-transfer}.
   *
   * Requirements:
   *
   * - `recipient` cannot be the zero address.
   * - the caller must have a balance of at least `amount`.
   */
  function transfer(address recipient, uint256 amount)
    public
    virtual
    override
    returns (bool)
  {
    _transfer(_msgSender(), recipient, amount);
    return true;
  }

  /**
   * @dev See {IERC20-allowance}.
   */
  function allowance(address owner, address spender)
    public
    view
    virtual
    override
    returns (uint256)
  {
    return _allowances[owner][spender];
  }

  /**
   * @dev See {IERC20-approve}.
   *
   * Requirements:
   *
   * - `spender` cannot be the zero address.
   */
  function approve(address spender, uint256 amount)
    public
    virtual
    override
    returns (bool)
  {
    _approve(_msgSender(), spender, amount);
    return true;
  }

  /**
   * @dev See {IERC20-transferFrom}.
   *
   * Emits an {Approval} event indicating the updated allowance. This is not
   * required by the EIP. See the note at the beginning of {ERC20}.
   *
   * Requirements:
   *
   * - `sender` and `recipient` cannot be the zero address.
   * - `sender` must have a balance of at least `amount`.
   * - the caller must have allowance for ``sender``'s tokens of at least
   * `amount`.
   */
  function transferFrom(
    address sender,
    address recipient,
    uint256 amount
  ) public virtual override returns (bool) {
    _transfer(sender, recipient, amount);

    uint256 currentAllowance = _allowances[sender][_msgSender()];
    require(
      currentAllowance >= amount,
      "ERC20: transfer amount exceeds allowance"
    );
    _approve(sender, _msgSender(), currentAllowance - amount);

    return true;
  }

  /**
   * @dev Atomically increases the allowance granted to `spender` by the caller.
   *
   * This is an alternative to {approve} that can be used as a mitigation for
   * problems described in {IERC20-approve}.
   *
   * Emits an {Approval} event indicating the updated allowance.
   *
   * Requirements:
   *
   * - `spender` cannot be the zero address.
   */
  function increaseAllowance(address spender, uint256 addedValue)
    public
    virtual
    returns (bool)
  {
    _approve(
      _msgSender(),
      spender,
      _allowances[_msgSender()][spender] + addedValue
    );
    return true;
  }

  /**
   * @dev Atomically decreases the allowance granted to `spender` by the caller.
   *
   * This is an alternative to {approve} that can be used as a mitigation for
   * problems described in {IERC20-approve}.
   *
   * Emits an {Approval} event indicating the updated allowance.
   *
   * Requirements:
   *
   * - `spender` cannot be the zero address.
   * - `spender` must have allowance for the caller of at least
   * `subtractedValue`.
   */
  function decreaseAllowance(address spender, uint256 subtractedValue)
    public
    virtual
    returns (bool)
  {
    uint256 currentAllowance = _allowances[_msgSender()][spender];
    require(
      currentAllowance >= subtractedValue,
      "ERC20: decreased allowance below zero"
    );
    _approve(_msgSender(), spender, currentAllowance - subtractedValue);

    return true;
  }

  /**
   * @dev Moves tokens `amount` from `sender` to `recipient`.
   *
   * This is internal function is equivalent to {transfer}, and can be used to
   * e.g. implement automatic token fees, slashing mechanisms, etc.
   *
   * Emits a {Transfer} event.
   *
   * Requirements:
   *
   * - `sender` cannot be the zero address.
   * - `recipient` cannot be the zero address.
   * - `sender` must have a balance of at least `amount`.
   */
  function _transfer(
    address sender,
    address recipient,
    uint256 amount
  ) internal virtual {
    require(sender != address(0), "ERC20: transfer from the zero address");
    require(recipient != address(0), "ERC20: transfer to the zero address");

    _beforeTokenTransfer(sender, recipient, amount);

    uint256 senderBalance = _balances[sender];
    require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
    _balances[sender] = senderBalance - amount;
    _balances[recipient] += amount;

    emit Transfer(sender, recipient, amount);
  }

  /** @dev Creates `amount` tokens and assigns them to `account`, increasing
   * the total supply.
   *
   * Emits a {Transfer} event with `from` set to the zero address.
   *
   * Requirements:
   *
   * - `account` cannot be the zero address.
   */
  function _mint(address account, uint256 amount) internal virtual {
    require(account != address(0), "ERC20: mint to the zero address");

    _beforeTokenTransfer(address(0), account, amount);

    _totalSupply += amount;
    _balances[account] += amount;
    emit Transfer(address(0), account, amount);
  }

  /**
   * @dev Destroys `amount` tokens from `account`, reducing the
   * total supply.
   *
   * Emits a {Transfer} event with `to` set to the zero address.
   *
   * Requirements:
   *
   * - `account` cannot be the zero address.
   * - `account` must have at least `amount` tokens.
   */
  function _burn(address account, uint256 amount) internal virtual {
    require(account != address(0), "ERC20: burn from the zero address");

    _beforeTokenTransfer(account, address(0), amount);

    uint256 accountBalance = _balances[account];
    require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
    _balances[account] = accountBalance - amount;
    _totalSupply -= amount;

    emit Transfer(account, address(0), amount);
  }

  /**
   * @dev Sets `amount` as the allowance of `spender` over the `owner` s tokens.
   *
   * This internal function is equivalent to `approve`, and can be used to
   * e.g. set automatic allowances for certain subsystems, etc.
   *
   * Emits an {Approval} event.
   *
   * Requirements:
   *
   * - `owner` cannot be the zero address.
   * - `spender` cannot be the zero address.
   */
  function _approve(
    address owner,
    address spender,
    uint256 amount
  ) internal virtual {
    require(owner != address(0), "ERC20: approve from the zero address");
    require(spender != address(0), "ERC20: approve to the zero address");

    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
  }

  /**
   * @dev Hook that is called before any transfer of tokens. This includes
   * minting and burning.
   *
   * Calling conditions:
   *
   * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
   * will be to transferred to `to`.
   * - when `from` is zero, `amount` tokens will be minted for `to`.
   * - when `to` is zero, `amount` of ``from``'s tokens will be burned.
   * - `from` and `to` are never both zero.
   *
   * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
   */
  function _beforeTokenTransfer(
    address from,
    address to,
    uint256 amount
  ) internal virtual {}
}

```

# 源码解读 

## SPDX License 标识

```
// SPDX-License-Identifier: MIT

```

智能合约信任的基础是建立在合约代码可见透明的基础上，会涉及到版权问题，如果你的合约代码开源可见，你可在文件头顶部添加以上注释。

## 编译版本声明

```
pragma solidity ^0.8.0;
```

此段代码是声明表示，源文件会被`0.8.0`以上版本的编译器编译。当然同时也不会对`0.9.0`以上版本的编译器起作用（这是由^符号来决定的）。`0.8.0` ~ `0.8.9`这些版本支持上面代码声明的源码编译，这样处理的好处是，如果`0.8.0`编译器有问题，可以随时修复bug，将其调整为`0.8.1`。

## 导入其他源文件

```
import "./IERC20.sol";
import "./IERC20Metadata.sol";
import "./Context.sol";
```

## 合约继承

```
contract ERC20 is Context, IERC20, IERC20Metadata {}
```

此段代码是声明一个名为 ERC20 的合约，当前合约继承自 Context、IERC20、IERC20Metadata 这三个合约，这里的继承关系和js中类的继承关系类似，子类可以调用父类的方法也可以重写父类的方法，通过 is 关键字实现继承。

## 基本变量

```
mapping(address => uint256) private _balances;

mapping(address => mapping(address => uint256)) private _allowances;

uint256 private _totalSupply;

string private _name;
string private _symbol;
```

此段代码是声明变量，Token 作为一个代币的称谓，自然少不了发行总量(totalSupply)、名称(name)、标识符(symbol)、余额(balances)等变量；mapping、uint256、string 表示变量的数据类型。

## constructor 初始化

```
constructor(
  string memory name_,
  string memory symbol_,
  uint256 totalSupply
) {
  _name = name_;
  _symbol = symbol_;
  _mint(_msgSender(), totalSupply);
}
```

此段代码是初始化合约，智能合约中的构造函数与 js 中构造函数类似，都用于初始化(即智能合约部署时)操作，智能合约中的构造函数有两种写法，一种是通过定义一个和合约名称一致的 public 型函数，另一种是通过关键字 constructor 来声明。

## 查询函数

```
function name() public view virtual override returns (string memory) {
  return _name;
}

function symbol() public view virtual override returns (string memory) {
  return _symbol;
}

function decimals() public view virtual override returns (uint8) {
  return 18;
}

function totalSupply() public view virtual override returns (uint256) {
  return _totalSupply;
}

function balanceOf(address account)
  public
  view
  virtual
  override
  returns (uint256)
{
  return _balances[account];
}

function allowance(address owner, address spender)
  public
  view
  virtual
  override
  returns (uint256)
{
  return _allowances[owner][spender];
}

```

此段代码是表示进行代币查询等；在智能合约中也可以对数据进行增删改查的基本操作，在ERC-20中提供了代币名称查询、代币标识符查询、代币精度查询、代币总额查询、用户资产查询、授信转账映射表查询。

## 转账逻辑

```
function transfer(address recipient, uint256 amount)
  public
  virtual
  override
  returns (bool)
{
  _transfer(_msgSender(), recipient, amount);
  return true;
}

function _transfer(
  address sender,
  address recipient,
  uint256 amount
) internal virtual {
  require(sender != address(0), "ERC20: transfer from the zero address");
  require(recipient != address(0), "ERC20: transfer to the zero address");

  _beforeTokenTransfer(sender, recipient, amount);

  uint256 senderBalance = _balances[sender];
  require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
  _balances[sender] = senderBalance - amount;
  _balances[recipient] += amount;

  emit Transfer(sender, recipient, amount);
}

function _beforeTokenTransfer(
  address from,
  address to,
  uint256 amount
) internal virtual {}
```

此段代码表示调用合约进行代币转账。调用 transfer 函数转账时需要提供以下三个参数：

* sender：函数的调用者，也是发送代币的地址
* recipient：代币接受地址
* amount：要转账的数量

调用 transfer 函数内的 _transfer 函数来实现转账操作：
* 检查发送者和接受者的地址是否为空(zero address)；address(0) 表示发送者和接受者不可为0地址，此处涉及显示转换，将数字转换为地址，防止因为地址没有填写而造成的错误转账；
* _beforeTokenTransfer 表示转帐前需要做的事情，也可以直接抛出异常来中断转账函数，相当于非主线逻辑全部使用异常来解决。同理，位于最下面的 _afterTokenTransfer 表示转账成功后要做的事情。_transfer 是一个非常重要的函数， 重新编写可能会不安全，所以最好不要重写。 解决方案是重写 _beforeTokenTransfer 函数，这是一个挂钩函数。 您可以重写此函数，之后每次转账都会调用它；
* 通过"_balances[sender]"来查询当前发送者用户所持有的资产数量，然后检查余额是否充足(检查发送者所持有的 token 数量是否大于等于要发送的 token 数量)；
* 更新发送者用户资产数量，更新代币接受地址资产数量；
* 通过 emit 关键字触发事件记录转账操作。

## 授权转账

```
function transferFrom(
  address sender,
  address recipient,
  uint256 amount
) public virtual override returns (bool) {
  _transfer(sender, recipient, amount);

  uint256 currentAllowance = _allowances[sender][_msgSender()];
  require(
    currentAllowance >= amount,
    "ERC20: transfer amount exceeds allowance"
  );
  _approve(sender, _msgSender(), currentAllowance - amount);

  return true;
}
```

此段代码表示合约中另一种业务逻辑"授权转账"，与转账逻辑不同的是"授权转账"逻辑虽然也是转账操作，但是它并非直接从A账户转账到C账户，而是从A账户授予可操作的代币额度到B账户，再由B账户操作将A账户的代币转账到C账户。

可以看到 transferFrom 函数与 transfer 函数首先不同的就是参数的个数不同，transferFrom 调用需要提供三个参数：
* sender：授权账户地址
* recipient：代币接受地址
* amount：要转的代币数量

调用 transferFrom 函数：
* 调用 _transfer 函数实现转账操作；
* 通过"_allowances[sender][_msgSender()]"获取 sender 账户的授权额度; 检查 sender 账户授权给函数调用者转账的额度，授权额度需大于等于当前转账额度，否则将回滚交易；
* 调用 _approve 函数将 sender 账户的授权额度更新。

## 授权额度

```
function approve(address spender, uint256 amount)
  public
  virtual
  override
  returns (bool)
{
  _approve(_msgSender(), spender, amount);
  return true;
}

function _approve(
  address owner,
  address spender,
  uint256 amount
) internal virtual {
  require(owner != address(0), "ERC20: approve from the zero address");
  require(spender != address(0), "ERC20: approve to the zero address");

  _allowances[owner][spender] = amount;
  emit Approval(owner, spender, amount);
}

function increaseAllowance(address spender, uint256 addedValue)
  public
  virtual
  returns (bool)
{
  _approve(
    _msgSender(),
    spender,
    _allowances[_msgSender()][spender] + addedValue
  );
  return true;
}

function decreaseAllowance(address spender, uint256 subtractedValue)
  public
  virtual
  returns (bool)
{
  uint256 currentAllowance = _allowances[_msgSender()][spender];
  require(
    currentAllowance >= subtractedValue,
    "ERC20: decreased allowance below zero"
  );
  _approve(_msgSender(), spender, currentAllowance - subtractedValue);

  return true;
}
```

此段代码表示申请授权额度(approve、_approve)、增加授权额度(increaseAllowance)、减少授权额度(decreaseAllowance)；

在 transferFrom 中我们有提及授权转账额度检查；调用函数 approve ，且在函数中调用 _approve 函数实现设置授权转账额度；

调用 approve 函数代币授权操作时需要提供以下两个参数：
* spender：申请授权代币的接受地址
* amount：申请授权代币的额度

调用 _approve 函数申请代币授权时需要提供以下三个参数：
* owner：合约的部署者，转交权限的地址
* spender：申请授权代币的接受地址
* amount：申请授权额度

调用 _approve 函数：
* 对转交权限的地址以及接受权限的地址进行非0检查(address(0))；
* 通过 _allowances 映射表来指定 owner 给 spender 地址用户赋予了转移多少代币的权限；
* 通过关键词 emit 触发 Approval 事件记录授权操作。

在以上代码还能看到两个用于增加(increaseAllowance)和减少(decreaseAllowance)授权额度的函数；此函数主要用于在原有的授权基础之上再增加授权额度或者减少授权额度；并且在调用 decreaseAllowance 函数减少授权额度时，会先通过 _allowances 映射表"_allowances[_msgSender()][spender]"获取授权额度，然后检查授权的余额是否充足(检查操作者所持有的授权额度是否大于等于要减少的额度)。

## 铸造代币

```
function _mint(address account, uint256 amount) internal virtual {
  require(account != address(0), "ERC20: mint to the zero address");

  _beforeTokenTransfer(address(0), account, amount);

  _totalSupply += amount;
  _balances[account] += amount;
  emit Transfer(address(0), account, amount);
}
```

此段代码是铸币函数，该函数主要用于增发代币操作，在合约初始化(即部署合约)时会触发此函数。

调用 _mint 函数需要传递两个参数：
* account：铸币时代币接受地址
* amount：铸币数量

调用 _mint 函数：
* 对代币的接受地址进行非0检查(address(0))；
* 增加代币总量；然后向函数调用地址 account 增发 amount 数量的代币；
* 通过 emit 关键字触发事件记录转账操作。

## 销毁代币

```
function _burn(address account, uint256 amount) internal virtual {
  require(account != address(0), "ERC20: burn from the zero address");

  _beforeTokenTransfer(account, address(0), amount);

  uint256 accountBalance = _balances[account];
  require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
  _balances[account] = accountBalance - amount;
  _totalSupply -= amount;

  emit Transfer(account, address(0), amount);
}
```

此段代码是代币销毁函数。

调用 _burn 函数需要传递两个参数：
* account：要销毁代币的地址
* amount：要销毁代币的数量

调用 _burn 函数：
* 对要销毁代币的地址进行非0检查(address(0))；
* 获取当前要销毁代币地址账户中可用的代币总量；检查可用余额是否大于要销毁的代币总量；
* 更新 account 地址账户的代币总量并且更新发行的 token 总量；
* 通过 emit 关键字触发事件记录转账操作。