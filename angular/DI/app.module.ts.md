```javascript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { ReactiveFormsModule } from '@angular/forms';
import { AppComponent } from './app.component';
import { MediaItemComponent } from './components/media-item.component';
import { MediaItemListComponent } from './components/media-item-list.component';
import { MediaItemFormComponent } from './components/media-item-form.component';
import { FavoriteDirective } from './favorite.directive';
import { CategoryListPipe } from './category-list.pipe';
import { lookupListToken, lookupLists } from './providers';

@NgModule({
  imports: [
    BrowserModule,
    ReactiveFormsModule
  ],
  declarations: [
    AppComponent,
    MediaItemComponent,
    MediaItemListComponent,
    MediaItemFormComponent,
    FavoriteDirective,
    CategoryListPipe,
  ],
  providers: [
    { provide: lookupListToken, useValue: lookupLists }
  ],
  bootstrap: [
    AppComponent
  ]
})
export class AppModule {}
```
