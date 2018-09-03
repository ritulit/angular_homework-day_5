#   Ajax request in angular

* Sensd a get request to this url: https://reqres.in/api/users?page=2   

and get the following response:
```json
{
    "page": 2,
    "per_page": 3,
    "total": 12,
    "total_pages": 4,
    "data": [{
        "id": 4,
        "first_name": "Eve",
        "last_name": "Holt",
        "avatar": "https://s3.amazonaws.com/uifaces/faces/twitter/marcoramires/128.jpg"
    }, {
        "id": 5,
        "first_name": "Charles",
        "last_name": "Morris",
        "avatar": "https://s3.amazonaws.com/uifaces/faces/twitter/stephenmoon/128.jpg"
    }, {
        "id": 6,
        "first_name": "Tracey",
        "last_name": "Ramos",
        "avatar": "https://s3.amazonaws.com/uifaces/faces/twitter/bigmancho/128.jpg"
    }]
}
```
* Convert the json response to ts interfaces with http://json2ts.com/   
The generated interfaces are:
```typescript
    export interface User {
        id: number;
        first_name: string;
        last_name: string;
        avatar: string;
    }

    export interface Users {
        page: number;
        per_page: number;
        total: number;
        total_pages: number;
        data: User[];
    }
```
* The goal is to create an angular project that sends an ajax request to the url that we used in the first step, and store the response into the interfaces that we created in the second step
* Create a new angular project with this command:
```bash
ng new project
```
* Create a sub folder named `shared` in `\project\src\app` - this folder will contain all the parts that are shared between all the components (services, models, etc...)
* Create a sub folder named `models` in `\project\src\app\shared` - this folder will contain all the classes / interfaces / enums 
* Create a new file named `user.model.ts`  in `\project\src\app\shared\models`, with the following content:
```typescript
    export interface User {
        id: number;
        first_name: string;
        last_name: string;
        avatar: string;
    }

```
* Create a new file named `users.model.ts`  in `\project\src\app\shared\models`, with the following content:
```typescript
import { User } from "./user.model";

    export interface Users {
        page: number;
        per_page: number;
        total: number;
        total_pages: number;
        data: User[];
    }
```
* Add to the `\project\src\app\app.module.ts`:
```typescript
import {HttpClientModule} from '@angular/common/http';
```
and then add to the `imports` array in the following module:
`HttpClientModule`

* Create a sub folder named `services` in `\project\src\app\shared` - this folder will contain all the services

* Create a new file named `user-request.service.ts`  in `\project\src\app\shared\services`, with the following content:
```typescript
import { HttpClient } from "@angular/common/http";
import { Users } from "../models/users.model";
import { Injectable } from "@angular/core";

@Injectable()
export class UserRequestService {

  userData: { users: Users } = {
    users: undefined
  };

  constructor(private myHttp: HttpClient) {
    this.myHttp.get("https://reqres.in/api/users?page=2")
    .subscribe(
        (response:Users)=>{this.userData.users=response;}
    );
  }
}
```
* Add to the `\project\src\app\app.module.ts`:
```typescript
import { UserRequestService } from './shared/services/user-request.service';
```
and then add to the `providers` array in the following module:
`UserRequestService`

* Edit the ts class of AppComponent (`\project\src\app\app.component.ts`) to this content
```typescript
import { Component } from "@angular/core";
import { UserRequestService } from "./shared/services/user-request.service";
import { Users } from "./shared/models/users.model";

@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  localData: { users: Users };

  constructor(private myService: UserRequestService) {
    this.localData = this.myService.userData;
  }
}
```
* Edit the template of AppComponent (`\project\src\app\app.component.html`) to this content
```html
<div *ngIf="localData && localData.users">
  <table border="1">
    <tr>
      <th>id</th>
      <th>first name</th>
      <th>last name</th>
      <th>avatar</th>
    </tr>
    <tr *ngFor="let user of localData.users.data">
      <td>{{user.id}}</td>
      <td>{{user.first_name}}</td>
      <td>{{user.last_name}}</td>
      <td><img [src]="user.avatar" /></td>
    </tr>
  </table>
</div>
```

## Project goals
* creating and running basic angular project
* creating new components and rendering into html 
* using one way binding - from html to ts- `{{}}`
* using one way binding - from ts to html - `()`
* Service + DI
* HttpClient


## Running the project (on local mode)

* Run `npm init` to install all the requiered packages from `package.json`
* Run `ng serve` to visit this site at `localhost:4200`

## See live demo
https://fierce-sands-82874.herokuapp.com/ajax# angular_homework-day_5
