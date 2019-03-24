# CodelabGoogleAnalytics

![Imgur](https://i.imgur.com/a0pURo7.png)

## 1. Create project

```
ng new codelab-google-analytics
```


## 2. Create pages

```
ng g c home
ng g c about
ng g c contact
```

## 3. Create routing

```
const routes: Routes = [
  {
    path: '', component: HomeComponent
  },
  {
    path: 'about', component: AboutComponent
  },
  {
    path: 'contact', component: ContactComponent
  }
];
```

```
<ul>
  <li>
    <h2><a routerLink="/">Home</a></h2>
  </li>
  <li>
    <h2><a routerLink="/about">About</a></h2>
  </li>
  <li>
    <h2><a routerLink="/contact">Contact</a></h2>
  </li>
</ul>

<router-outlet></router-outlet>

```

### 3 Add google code

```
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=xxxxx"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'xxxxx');
</script>
```

### 4 Observer navigation ends

```
const navEndEvents$ = this.router.events
.pipe(
  filter(event => event instanceof NavigationEnd)
);

navEndEvents$.subscribe((event: NavigationEnd) => {
  gtag('config', 'xxxxxx', {
    'page_path': event.urlAfterRedirects
  });
});
```

```
import { Component } from '@angular/core';
import { Router, NavigationEnd } from '@angular/router';

import { filter } from 'rxjs/operators';

declare var gtag;

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'codelab-google-analytics';

  constructor(
    private router: Router
  ) {
    const navEndEvents$ = this.router.events
    .pipe(
      filter(event => event instanceof NavigationEnd)
    );

    navEndEvents$.subscribe((event: NavigationEnd) => {
      gtag('config', 'UA-136822854-1', {
        'page_path': event.urlAfterRedirects
      });
    });
  }
}

```