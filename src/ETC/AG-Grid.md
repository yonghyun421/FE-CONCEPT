# AG-Grid란?

자바스크립트 기반의 오픈 소스 그리드.
서버사이드 렌더링, 엑셀 추출, Master-Detail 구조, Tree, Pivot 등을 지원하며 무료버전과 유료버전으로 나뉘어 있지만 무료 버전만으로도 많은 기능을 제공하고 있다.

## AG-Grid 사용

JS 스크립트 기반으로 어떤 플랫폼에서도 사용할 수 있다.
복잡한 그리드를 Ag-grid를 통해 손쉽게 그릴 수 있으며 수많은 데이터를 한눈에 보기 쉽게 정리할 수 있다는 장점이 있다.

### AG-Grid 설치하기

```bash
npm install ag-grid-community ag-grid-react --save
```

### Column 생성

```js
import React from "react";
import { render } from "react-dom";
import { AgGridColumn, AgGridReact } from "ag-grid-react";

import "ag-grid-community/dist/styles/ag-grid.css";
import "ag-grid-community/dist/styles/ag-theme-alpine.css";

const App = () => {
  const rowData = [
    { make: "Toyota", model: "Celica", price: 35000 },
    { make: "Ford", model: "Mondeo", price: 32000 },
    { make: "Porsche", model: "Boxter", price: 72000 },
  ];

  return (
    <div className="ag-theme-alpine" style={{ height: 400, width: 600 }}>
      <AgGridReact rowData={rowData}>
        <AgGridColumn field="make"></AgGridColumn>
        <AgGridColumn field="model"></AgGridColumn>
        <AgGridColumn field="price"></AgGridColumn>
      </AgGridReact>
    </div>
  );
};

render(<App />, document.getElementById("root"));
```

AgGridReact와 필요한 CSS를 import한다.
그리드의 두 가지 필수 구성 속성인 열 정의(columnDefs)와 데이터(rowData)는 필수로 들어가야 한다.

## AG-Grid의 기능

### 검색 및 정렬

ColumnDefs에 저장된 여러개의 컬럼들을 공통의 기본값으로 정의하기 위해서는 defaultColDef 속성을 사용해야 한다.
defaultColDef에 정의 된 속성들을 살펴보면

- sortable : 열 머리글을 클릭하여 그리드를 정렬할 수 있는 기능으로, 머리글을 클릭하면 오름차순, 내림차순 및 정렬 기본으로 전환된다. 실제 응용 프로그램에서 많은 열과 수많은 행이 있을 경우 정렬해주는 기능은 필수이다.
- filter : 검색 기능. 머리글을 가리키면 그리드에 작은 열 메뉴 아이콘이 표시된다. 그것을 누르면 필터의 종류와 필터링할 텍스트를 선택할 수 있는 필터링 UI가 있는 팝업이 표시된다.
- floatingFilter : 검색을 하기 위해 머리글을 한 번 클릭하고 검색하는 것은 불편하다. header 밑행에 바로 검색할 수 있게 설정한다.
- resizeable : header의 사이즈 간격 조절이 가능하게 해 준다.

### Axios를 통한 CRUD

### TreeData

treeData는 부모와 자식 관계로 이루어진 상위/하위 데이터로 상위 폴더 한 개에 여러 개의 자식을 넣을 수 있다.
비슷한 성격을 가진 rowGroup은 행을 그룹화를 하여 여러 개의 열을 표현할 수 있다.

## AG-Grid의 장단점

### 장점

- 무료지만 막강한 기능 제공을 하며 상용버전도 저렴하다.
- 자바스크립트 기반으로 어떤 플랫폼에서도 사용이 가능하다.
- 설치가 쉽다.
- 사용하기 쉽다.
- 자세한 레퍼런스와 샘플, 코드들을 제공한다.
- 확장성이 좋기 때문에 사용하는 개발자의 역량에 따라 서비스에 맞는 컴포넌트 등을 추가 개발하여 이용하기 쉽다.

### 단점

- 그리드만 제공하기 때문에 차트와 그래프는 적당한걸 찾아서 별도로 이용해야 한다.
- 한글 가이드문서가 없다.
- 참고 레퍼런스가 부족하다.
