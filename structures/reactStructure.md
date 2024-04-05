# React/Next.js practice structure

```
.
├── 📁 commons/
│   ├── 📁 Navbar/
│   │   ├── index.tsx/jsx
│   │   └── Navbar.module.css
│   ├── 📁 NavbarMobile/
│   │   ├── index.tsx/jsx
│   │   └── NavbarMobile.module.css
│   └── 📁 Button/
│       ├── index.tsx/jsx
│       └── Button.module.css
├── 📁 featureComponents/
│   └── 📁 GeneratedComponent/
│       ├── index.tsx/jsx
│       └── GeneratedComponent.module.css
├── 📁 pages/
│   ├── 📁 Home/
│   │   ├── index.tsx/jsx
│   │   └── Home.module.css
│   └── 📁 ExamplePage/
│       ├── index.tsx/jsx
│       └── ExamplePage.module.css
├── 📁 contexts/
│   └── ExampleContext.tsx/jsx
├── 📁 utils/
│   └── dateFormat.ts/js
├── 📁 constants/
│   ├── color.constant.ts/js
│   └── url.constanst.ts/js
├── 📁 models/
│   ├── 📁 types/
│   │   ├── promptInput.type.ts/js
│   │   └── user.type.ts/js
│   └── 📁 interfaces/
│       ├── SampleContext.interface.ts/js
│       └── SamplePageProps.interface.ts/js
└── 📁 services/
    └── 📁 api/
        ├── SampleApi.ts/js
        └── BookApi.ts/js
```

### Name convension
* Component, Page component's name, Component interface/type use Pascal case ex. `SamplePage`, `GenerateButton`
* Variables, functions and CSS classes use Camel case ex. `getData`, `language`, `generatedButton`, `redColor`, `mainContainer`


### React code pattern
#### Function declaration
* Use Camel case for function's name ex. `getData`, `formatDate`
* Use classical function
```typescript
// formatPhone.ts/js
function formatPhoneNumber(phoneNumber: string): string {
  return phoneNumber && phoneNumber.length === 10
    ? `${phoneNumber.substring(0, 2)}-${phoneNumber.substring(2, 6)}-${phoneNumber.substring(6)}`
    : phoneNumber;
}
```

#### Component
* To create new component
  - Create folder name with component's name.
  - In folder, create file `index.tsx/jsx` for component's code
  - For css file, create file `[ComponentName].module.css` in same level as index file.
* Component declaration, function inside component use "arrow function"

##### Example
```
📁 SamplePage/
    ├── index.tsx/jsx
    └── SamplePage.module.css
```

##### Code pattern
* Component's code apply "Container and presentation pattern"
```typescript
import React, { useEffect } from "react";
import { apiGetSampleData } from "@/services/api/SampleService"

const SamplePage = () => {
  const [sampleData, setSampleData] = useState<Sample>(false);
  const [error, setError] = useState<boolean>(false);
  
  const fetchData = async () => {
    try {
      const response = await apiGetSampleData()
      if (response.status === 200) {
        setSampleData(response.data)
      }
    } catch (err) {
      console.error(err)
    }
  }  
  useEffect(() => {
    fetchData()
  }, []);

  return (
    <div>
      <SampleHeader />
      <SampleTable data={sampleData}/>
      <SampleFooter />
    </div>
  );
};

export default SamplePage;
```

#### API call function
* Always use try...catch statement for error handling.
* File name is `[Name]Api.ts/js` ex. `LoginApi.ts`
```typescript
import requestOption from "@/constants/requestOption.constant"
import apiUrl from "@/constants/url.constant"

export async function getSampleData() {
  try {
    const accessToken = await getAccessToken()
    const response = await axios.get(
      apiUrl,
      requestOption
    );

    return response
  } catch (error) {
    console.error(error);
    return { reply: 'Error Please try again' }
  }
}
```
#### Type and Interface
##### Type
* File name: `book.model.ts/js`, `sampleRequest.model.ts/js`
* Type is for data object or model ex. Request, Response.
* Use keyword `type` for type declaration. 
```typescript
// sample.type.ts/js
export type Sample = {
  ID: string
  name: string
  description?: string
}
```

##### Interface
* File name: `sample.model.ts/js`
* Interface is for Component structure; type of parameters and functions.
* Use keyword `interface` for interface declararion.
```typescript
import Sample from "@/models/Sample"
export interface SampleContextInteface {
  data: []Sample
  handleOpen: () => void 
}
```

#### Context
* Context is utilized for provide common data to component such as signed in user, screen resolution.
```typescript
import SampleContextInteface from "@/interfaces/SampleContextInteface"
import React, { createContext, useContext, useState } from 'react';

const SampleContext = createContext<SampleContextInteface | undefined>(undefined);

interface Props {
   children: ReactNode;
}

export function useSampleContext = () => useContext(SampleContext);

export const SampleContextProvider = ({ children }) => {
  const [value, setValue] = useState('initial value');

  const contextValue: SampleContextInteface = {
    value: value || null,
  }

  return (
    <MyContext.Provider value={contextValue}>
      {children}
    </MyContext.Provider>
  );
};
```