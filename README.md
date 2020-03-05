##### 1. Wrapper - Page:
Ở một Wrapper khi render Page thì phải truyền props `sharedFunctions` như sau:
```jsx
export default class Wrapper extends BaseRouteWrapper {
    componentDidMount() {
        this.setData({});
    }
    wrapperContent() {
        const data = this.getData();
        const { sharedFunctions } = this.props;
        return (
            <Page 
                data={data}
                sharedFunctions={sharedFunctions}
            />
        )
    }
}
```
##### 2. Shared functions
- Công dụng của props này là để ở các Page có thể thực hiện được các hàm ở AppRoot do AppRoot truyền vào (việc thực hiện này được implement ở PageLayout), ví dụ:
    Ở trang `HomePage` có override page title là `this.pageTitle = "Trang chủ"` khi đó ở did mount của `PageLayout` thực hiện hành động như sau:
```jsx
    componentDidMount() {
        document.title = this.pageTitle; // cái này là set cái title của trình duyệt thôi (ko cần quan tâm)
        const { setPageTitle } = this.props.sharedFunctions;
        // hàm setPageTitle được truyền vào từ AppRoot, vì AppRoot giữ pageTitle nên [việc set page title] là do AppRoot làm
        // nhưng [việc set page title là gì và thực hiện việc đó khi nào] thì do các Page quyết định 
        typeof setPageTitle === 'function' && setPageTitle(this.pageTitle); 
    }
```
- Ngoài ra còn có một số hàm khác, về cơ bản cơ chế giống set page title ở trên

##### 3. Page component
- Page cần kế thừa `PageLayout` override hàm render là `renderBody` 
- Ngoài ra có thể override hàm `renderUnderHeader` để render phần có nội dung bên dưới header (background xanh), xem VD
```jsx
class Page extends PageLayout {
    constructor(props) {
        super(props);
        this.pageTitle = "Hoạt động";
        this.tabbarValue = Enum.TabType.Activity;
    }
    renderUnderHeader(){
        return (
            <div style={{padding: '1rem'}}>
                UNDER HEADER
            </div>
        )
    }
    renderBody(){
        return (
            <b>nội dung page</b>
        );
    }
}
```
![alt text](https://i.imgur.com/0RL3Tju.png)

- Override `this.pageTitle` (`string`) để set page title (đã nói ở phần 2)
- Override `this.tabbarValue` (`Enum.TabType`)  để set giá trị của tabbar khi vào trang đó
- Override `this.showTabbar`(`boolean`) để App biết Page đó có sử dụng Tabbar hay không

##### 4. BaseCreator
- Việc thực hiện một hành động như `openModal`, hoặc get dữ liệu, ... nên được implement ở 1 chỗ, việc tái sử dụng sẽ hiệu quả hơn
Ví dụ: open một modal bệnh án thì mỗi trang sẽ có cách hiển thị 'nút open bệnh án' khác nhau (button hoặc thẻ a), nhưng việc gọi `this.openModal` nên được tập trung 1 chỗ
```jsx
export default class BaseCreator extends BaseConsumer {
    static propTypes = {
        renderContent: PropTypes.func.isRequired
    }
    onClick() {
        throw "not implemented onClick"
    }
    consumerContent() {
        const { renderContent } = this.props;
        return renderContent(this.onClick);
    }
}
```
- Giả sử cần một component để mở bệnh án, implement như sau \
```jsx
export default class BenhAnModalCreator extends BaseCreator {
    onClick() {
        this.openModal(bla bla bla);
    }
}
```
- Cách sử dụng:
```jsx
<BenhAnModalCreator
    renderContent={(onClickFunc) => {
        return (
            <div onClick={onClickFunc}>
                nhấn vào đây để mở bệnh án
            </div>
        )
    }}
/>
```



