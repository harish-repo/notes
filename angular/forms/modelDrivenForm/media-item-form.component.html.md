```html
<header>
    <h2>Add Media to Watch</h2>
  </header>
  <form [formGroup]="form"
        (ngSubmit)="onSubmit(form.value)">
    <ul>
      <li>
        <label for="medium">Medium</label>
        <select name="medium" id="medium" formControlName="medium">
          <option value="Movies">Movies</option>
          <option value="Series">Series</option>
        </select>
      </li>
      <li>
        <label for="name">Name</label>
        <input type="text" name="name" id="name" formControlName="name">
        <div *ngIf="form.get('name').hasError('pattern')" class="error">Name has invalid character</div>
      </li>
      <li>
        <label for="category">Category</label>
        <select name="category" id="category" formControlName="category">
          <option value="Action">Action</option>
          <option value="Science Fiction">Science Fiction</option>
          <option value="Comedy">Comedy</option>
          <option value="Drama">Drama</option>
          <option value="Horror">Horror</option>
          <option value="Romance">Romance</option>
        </select>
      </li>
      <li>
        <label for="year">Year</label>
        <input type="text" name="year" id="year" maxlength="4" formControlName="year">
        <div *ngIf="form.get('year').errors as yearErrors" class="error">
            Must be between {{yearErrors.year.min}} and {{yearErrors.year.max}}</div>
      </li>
    </ul>
    <button type="submit" [disabled]="!form.valid">Save</button>
  </form>
```