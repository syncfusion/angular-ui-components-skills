# Appearance and Customization

## Table of Contents
- [Editor Dimensions](#editor-dimensions)
- [CSS Classes and Styling](#css-classes-and-styling)
- [Theme Customization](#theme-customization)
- [Custom CSS Variables](#custom-css-variables)
- [Font and Typography Customization](#font-and-typography-customization)
- [Color Schemes](#color-schemes)
- [Advanced Customization Examples](#advanced-customization-examples)

## Editor Dimensions

### Width and Height Configuration

Set custom dimensions for the editor:

```typescript
@Component({
  selector: 'app-root',
  template: `
    <ejs-blockeditor 
      [width]="editorWidth"
      [height]="editorHeight"
    />
  `,
  standalone: true,
  imports: [BlockEditorModule]
})
export class AppComponent {
  public editorWidth = '100%';      // Width (string or number)
  public editorHeight = '500px';    // Height (string or number)
}
```

### Responsive Sizing

```typescript
@Component({
  selector: 'app-responsive-editor',
  template: `
    <ejs-blockeditor 
      [width]="getWidth()"
      [height]="getHeight()"
    />
  `,
  standalone: true,
  imports: [BlockEditorModule]
})
export class ResponsiveEditorComponent {
  public getWidth(): string {
    const width = window.innerWidth;
    if (width < 768) return '100%';
    if (width < 1024) return '90%';
    return '80%';
  }

  public getHeight(): string {
    const height = window.innerHeight;
    return Math.max(400, height - 150) + 'px';
  }

  constructor() {
    window.addEventListener('resize', () => {
      // Trigger resize detection
    });
  }
}
```

### Min/Max Constraints

```typescript
public editorConfig = {
  width: '100%',
  height: '600px',
  minHeight: '300px',
  maxHeight: '1000px',
  minWidth: '200px',
  maxWidth: 'none'
};
```

## CSS Classes and Styling

### Default Editor Classes

```css
/* Main editor container */
.ej-blockeditor {
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  background-color: #fff;
}

/* Content area */
.ej-blockeditor .content {
  padding: 16px;
  min-height: 300px;
}

/* Individual block */
.ej-blockeditor .block {
  margin: 8px 0;
  padding: 4px 0;
  position: relative;
}

/* Block placeholder */
.ej-blockeditor .block-placeholder {
  color: #999;
  font-style: italic;
}

/* Toolbar */
.ej-blockeditor .toolbar {
  display: flex;
  gap: 4px;
  padding: 8px;
  border-bottom: 1px solid #e0e0e0;
}

/* Toolbar buttons */
.ej-blockeditor .toolbar-button {
  padding: 6px 12px;
  border: 1px solid #ddd;
  border-radius: 3px;
  cursor: pointer;
  background-color: #fff;
}

.ej-blockeditor .toolbar-button:hover {
  background-color: #f5f5f5;
}

.ej-blockeditor .toolbar-button.active {
  background-color: #e3f2fd;
  border-color: #2196f3;
  color: #2196f3;
}
```

### Custom Styling Example

```css
.custom-editor {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  line-height: 1.6;
  background-color: #fafafa;
}

.custom-editor .content {
  padding: 24px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.custom-editor .block {
  transition: background-color 0.2s ease;
}

.custom-editor .block:hover {
  background-color: #f5f5f5;
}

.custom-editor .toolbar {
  background-color: #f5f5f5;
  border-radius: 4px 4px 0 0;
}
```

## Theme Customization

### Theme Configuration

Define a custom theme:

```typescript
public themeConfig = {
  primary: '#2196F3',
  secondary: '#FF9800',
  background: '#fff',
  surface: '#f5f5f5',
  text: '#333',
  textSecondary: '#666',
  border: '#e0e0e0',
  error: '#F44336',
  warning: '#FFC107',
  success: '#4CAF50',
  info: '#2196F3'
};

@Component({
  selector: 'app-themed-editor',
  template: `<ejs-blockeditor [ngClass]="'theme-' + currentTheme" />`,
  styles: [`
    :host ::ng-deep .theme-dark {
      --primary: #90CAF9;
      --secondary: #FFCC80;
      --background: #121212;
      --surface: #1e1e1e;
      --text: #fff;
      --border: #333;
    }
  `],
  standalone: true,
  imports: [BlockEditorModule]
})
export class ThemedEditorComponent {
  public currentTheme = 'light';

  public setTheme(theme: 'light' | 'dark' | 'high-contrast'): void {
    this.currentTheme = theme;
    this.applyThemeStyles(theme);
  }

  private applyThemeStyles(theme: string): void {
    document.documentElement.setAttribute('data-theme', theme);
  }
}
```

## Custom CSS Variables

### Define CSS Variables

```css
:root {
  /* Color variables */
  --color-primary: #2196F3;
  --color-primary-light: #E3F2FD;
  --color-primary-dark: #1976D2;
  
  --color-secondary: #FF9800;
  --color-secondary-light: #FFE0B2;
  --color-secondary-dark: #F57C00;
  
  --color-background: #FFFFFF;
  --color-surface: #F5F5F5;
  --color-surface-variant: #EEEEEE;
  
  --color-text: #212121;
  --color-text-secondary: #666666;
  --color-text-disabled: #BDBDBD;
  
  --color-border: #E0E0E0;
  --color-divider: #BDBDBD;
  
  /* Spacing variables */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;
  
  /* Typography variables */
  --font-family-base: 'Segoe UI', Tahoma, Geneva, sans-serif;
  --font-family-mono: 'Monaco', 'Courier New', monospace;
  
  --font-size-sm: 12px;
  --font-size-base: 14px;
  --font-size-lg: 16px;
  --font-size-xl: 20px;
  --font-size-2xl: 24px;
  
  --line-height-tight: 1.2;
  --line-height-normal: 1.5;
  --line-height-loose: 1.8;
  
  /* Border variables */
  --border-radius-sm: 3px;
  --border-radius-md: 4px;
  --border-radius-lg: 8px;
  
  --border-width: 1px;
  --border-width-thick: 2px;
  
  /* Shadow variables */
  --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.12);
  --shadow-md: 0 2px 8px rgba(0, 0, 0, 0.15);
  --shadow-lg: 0 8px 16px rgba(0, 0, 0, 0.2);
}
```

### Use CSS Variables in Editor

```css
.ej-blockeditor {
  font-family: var(--font-family-base);
  background-color: var(--color-background);
  color: var(--color-text);
  border: var(--border-width) solid var(--color-border);
}

.ej-blockeditor .content {
  padding: var(--spacing-md);
  font-size: var(--font-size-base);
  line-height: var(--line-height-normal);
}

.ej-blockeditor .block {
  margin: var(--spacing-sm) 0;
  border-radius: var(--border-radius-sm);
}

.ej-blockeditor .toolbar {
  background-color: var(--color-surface);
  border-bottom: var(--border-width) solid var(--color-border);
  gap: var(--spacing-sm);
  padding: var(--spacing-sm);
}

.ej-blockeditor .toolbar-button {
  padding: var(--spacing-sm) var(--spacing-md);
  border-radius: var(--border-radius-md);
  color: var(--color-text);
  background-color: var(--color-background);
  border: var(--border-width) solid var(--color-border);
}

.ej-blockeditor .toolbar-button:hover {
  background-color: var(--color-surface-variant);
}

.ej-blockeditor .toolbar-button.active {
  color: var(--color-primary);
  background-color: var(--color-primary-light);
  border-color: var(--color-primary);
}
```

## Font and Typography Customization

### Font Configuration

```typescript
public fontSettings = {
  fontFamily: 'Arial, sans-serif',
  fontSize: 14,
  lineHeight: 1.5,
  fontWeight: 400,
  letterSpacing: '0px'
};

@Component({
  selector: 'app-typography-editor',
  template: `<ejs-blockeditor [ngClass]="fontClass" />`,
  styles: [`
    .serif-font {
      font-family: Georgia, serif;
    }
    
    .sans-serif-font {
      font-family: 'Trebuchet MS', sans-serif;
    }
    
    .monospace-font {
      font-family: 'Courier New', monospace;
    }
  `],
  standalone: true,
  imports: [BlockEditorModule]
})
export class TypographyEditorComponent {
  public fontClass = 'sans-serif-font';

  public setFont(font: 'serif' | 'sans-serif' | 'monospace'): void {
    this.fontClass = font + '-font';
  }
}
```

### Heading Styles

```css
.ej-blockeditor h1 {
  font-size: 32px;
  font-weight: 700;
  line-height: 1.2;
  margin-top: 24px;
  margin-bottom: 16px;
  color: var(--color-text);
}

.ej-blockeditor h2 {
  font-size: 28px;
  font-weight: 700;
  line-height: 1.3;
  margin-top: 20px;
  margin-bottom: 12px;
}

.ej-blockeditor h3 {
  font-size: 24px;
  font-weight: 600;
  line-height: 1.4;
  margin-top: 16px;
  margin-bottom: 10px;
}

.ej-blockeditor h4 {
  font-size: 20px;
  font-weight: 600;
  line-height: 1.5;
  margin-top: 12px;
  margin-bottom: 8px;
}

.ej-blockeditor p {
  font-size: 16px;
  line-height: 1.6;
  margin-bottom: 12px;
}

.ej-blockeditor code {
  font-family: 'Monaco', 'Courier New', monospace;
  font-size: 14px;
  background-color: var(--color-surface);
  padding: 2px 4px;
  border-radius: 3px;
}
```

## Color Schemes

### Light Theme

```typescript
export const LIGHT_THEME = {
  primary: '#2196F3',
  secondary: '#FF9800',
  background: '#FFFFFF',
  surface: '#F5F5F5',
  text: '#212121',
  textSecondary: '#666666',
  border: '#E0E0E0',
  error: '#F44336',
  success: '#4CAF50',
  warning: '#FFC107',
  info: '#2196F3'
};
```

### Dark Theme

```typescript
export const DARK_THEME = {
  primary: '#90CAF9',
  secondary: '#FFCC80',
  background: '#121212',
  surface: '#1E1E1E',
  text: '#FFFFFF',
  textSecondary: '#BDBDBD',
  border: '#424242',
  error: '#EF5350',
  success: '#81C784',
  warning: '#FFD54F',
  info: '#64B5F6'
};
```

### High Contrast Theme

```typescript
export const HIGH_CONTRAST_THEME = {
  primary: '#0000FF',
  secondary: '#FFFF00',
  background: '#FFFFFF',
  surface: '#F0F0F0',
  text: '#000000',
  textSecondary: '#333333',
  border: '#000000',
  error: '#FF0000',
  success: '#008000',
  warning: '#CCAA00',
  info: '#0000FF'
};
```

### Theme Service

```typescript
@Injectable({ providedIn: 'root' })
export class ThemeService {
  private themes = {
    light: LIGHT_THEME,
    dark: DARK_THEME,
    'high-contrast': HIGH_CONTRAST_THEME
  };

  private currentTheme$ = new BehaviorSubject<string>('light');

  public setTheme(theme: string): void {
    if (this.themes[theme as keyof typeof this.themes]) {
      this.currentTheme$.next(theme);
      this.applyTheme(this.themes[theme as keyof typeof this.themes]);
    }
  }

  public getTheme(name: string): any {
    return this.themes[name as keyof typeof this.themes];
  }

  private applyTheme(theme: any): void {
    const root = document.documentElement;
    Object.entries(theme).forEach(([key, value]) => {
      root.style.setProperty(`--color-${this.kebabCase(key)}`, value as string);
    });
  }

  private kebabCase(str: string): string {
    return str.replace(/([a-z0-9])([A-Z])/g, '$1-$2').toLowerCase();
  }
}
```

## Advanced Customization Examples

### Example 1: Custom Editor with Full Theming

```typescript
@Component({
  selector: 'app-themed-editor-full',
  template: `
    <div class="editor-wrapper" [attr.data-theme]="currentTheme">
      <div class="theme-selector">
        <button *ngFor="let theme of themes" 
                (click)="setTheme(theme)"
                [class.active]="currentTheme === theme">
          {{ theme | titlecase }}
        </button>
      </div>
      <ejs-blockeditor 
        #blockEditor
        class="custom-block-editor"
        [width]="'100%'"
        [height]="'600px'"
      />
    </div>
  `,
  styles: [`
    .editor-wrapper {
      padding: 20px;
    }

    [data-theme="light"] {
      --bg-primary: #fff;
      --text-primary: #333;
      --border-color: #ddd;
    }

    [data-theme="dark"] {
      --bg-primary: #1e1e1e;
      --text-primary: #fff;
      --border-color: #444;
    }

    [data-theme="high-contrast"] {
      --bg-primary: #fff;
      --text-primary: #000;
      --border-color: #000;
    }

    .custom-block-editor {
      background-color: var(--bg-primary);
      color: var(--text-primary);
      border: 2px solid var(--border-color);
    }

    .theme-selector {
      margin-bottom: 16px;
      display: flex;
      gap: 8px;
    }

    .theme-selector button {
      padding: 8px 16px;
      border: 1px solid var(--border-color);
      background-color: var(--bg-primary);
      color: var(--text-primary);
      cursor: pointer;
      border-radius: 4px;
      transition: all 0.3s ease;
    }

    .theme-selector button.active {
      background-color: var(--text-primary);
      color: var(--bg-primary);
    }
  `],
  standalone: true,
  imports: [BlockEditorModule, CommonModule]
})
export class ThemedEditorFullComponent {
  public currentTheme = 'light';
  public themes = ['light', 'dark', 'high-contrast'];

  constructor(private themeService: ThemeService) {}

  public setTheme(theme: string): void {
    this.currentTheme = theme;
    this.themeService.setTheme(theme);
  }
}
```

### Example 2: Responsive Custom Styling

```typescript
@Component({
  selector: 'app-responsive-styled',
  template: `<ejs-blockeditor [ngClass]="'editor-' + screenSize" />`,
  styles: [`
    /* Mobile */
    .editor-xs {
      width: 100%;
      height: 400px;
    }

    .editor-xs ::ng-deep .toolbar {
      flex-wrap: wrap;
    }

    /* Tablet */
    .editor-sm {
      width: 90%;
      height: 500px;
      margin: 0 auto;
    }

    /* Desktop */
    .editor-md,
    .editor-lg {
      width: 80%;
      height: 600px;
      margin: 0 auto;
    }

    /* Large Desktop */
    .editor-xl {
      width: 1200px;
      height: 700px;
      margin: 0 auto;
    }
  `],
  standalone: true,
  imports: [BlockEditorModule]
})
export class ResponsiveStyledComponent {
  public screenSize: 'xs' | 'sm' | 'md' | 'lg' | 'xl' = 'md';

  constructor() {
    this.updateScreenSize();
    window.addEventListener('resize', () => this.updateScreenSize());
  }

  private updateScreenSize(): void {
    const width = window.innerWidth;
    if (width < 576) this.screenSize = 'xs';
    else if (width < 768) this.screenSize = 'sm';
    else if (width < 992) this.screenSize = 'md';
    else if (width < 1200) this.screenSize = 'lg';
    else this.screenSize = 'xl';
  }
}
```

