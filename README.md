### 1. Wrapper - Page:
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
