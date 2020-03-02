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

##### 2. Page component
- Page cần override hàm render là `renderBody`
- Override hàm `setUnderHeader` nếu cần có phần nội dung bên dưới header, xem ví dụ bên dưới
```jsx
class ActivityPage extends PageLayout {
    constructor(props) {
        super(props);
        this.pageTitle = "Hoạt động";
        this.tabbarValue = Enum.TabType.Activity;
    }
    setUnderHeader(){
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
