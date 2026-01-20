<!-- Overlay backdrop -->
<div class="column-menu-overlay" *ngIf="showColumnMenu" (click)="closeColumnMenu()"></div>

<!-- Column management popup -->
<div class="column-menu" *ngIf="showColumnMenu">
    <div class="column-menu-header">
      <h3>Table Options</h3>
      <button mat-icon-button (click)="closeColumnMenu()" class="close-btn">
        <mat-icon>close</mat-icon>
      </button>
    </div>

    <mat-tab-group class="column-menu-tabs">
      <!-- Column Management Tab -->
      <mat-tab>
        <ng-template mat-tab-label>
          <mat-icon class="tab-icon">view_column</mat-icon>
          Columns
        </ng-template>
        
        <div class="tab-content">
          <!-- Column list with dropdowns -->
          <div class="column-list" cdkDropList (cdkDropListDropped)="dropColumn($event)">
            <div class="column-item" cdkDrag *ngFor="let column of displayedColumns; let i = index">
              <mat-icon class="drag-handle" cdkDragHandle>drag_indicator</mat-icon>
              <select 
                class="column-select"
                [ngModel]="displayedColumns[i]"
                (ngModelChange)="onColumnChange(i, $event)">
                <option *ngFor="let availableCol of getAvailableColumnsForDropdown(i)" 
                        [value]="availableCol">
                  {{ availableCol }}
                </option>
              </select>
              <button mat-icon-button 
                      class="remove-btn"
                      (click)="removeColumn(i)"
                      [matTooltip]="displayedColumns.length === 1 ? 'Cannot remove the last column' : 'Remove column'">
                <mat-icon>close</mat-icon>
              </button>
            </div>
          </div>

          <!-- Add column button -->
          <div class="add-column-section">
            <button mat-raised-button 
                    color="primary"
                    (click)="addNewColumn()"
                    [disabled]="getAvailableColumns().length === 0"
                    class="add-column-btn">
              <mat-icon>add</mat-icon>
              Add Column
            </button>
            <p class="available-info" *ngIf="getAvailableColumns().length > 0">
              {{ getAvailableColumns().length }} column(s) available
            </p>
            <p class="available-info no-columns" *ngIf="getAvailableColumns().length === 0">
              All columns are visible
            </p>
          </div>
        </div>
      </mat-tab>

      <!-- Sorting Tab -->
      <mat-tab>
        <ng-template mat-tab-label>
          <mat-icon class="tab-icon">sort</mat-icon>
          Sort
        </ng-template>
        
        <div class="tab-content">
          <div class="sorting-section">
            <h4>Multi-Column Sort</h4>
            
            <!-- Sort list for all displayed columns -->
            <div class="sort-criteria-list">
              <div class="sort-criteria-item" *ngFor="let column of displayedColumns; let i = index">
                <span class="column-name-label">{{ column }}</span>
                
                <button mat-icon-button 
                        [color]="getColumnSortDirection(column) === 'asc' ? 'primary' : 'default'"
                        (click)="toggleColumnSort(column, 'asc')"
                        matTooltip="Sort Ascending"
                        class="sort-btn">
                  <mat-icon>arrow_upward</mat-icon>
                </button>
                
                <button mat-icon-button 
                        [color]="getColumnSortDirection(column) === 'desc' ? 'primary' : 'default'"
                        (click)="toggleColumnSort(column, 'desc')"
                        matTooltip="Sort Descending"
                        class="sort-btn">
                  <mat-icon>arrow_downward</mat-icon>
                </button>
              </div>
            </div>

            <!-- Apply and Clear buttons -->
            <div class="sort-action-buttons">
              <button mat-raised-button 
                      color="primary"x
                      (click)="applyMultiColumnSort()"
                      [disabled]="sortCriteria.length === 0"
                      class="apply-sort-btn">
                <mat-icon>check</mat-icon>
                Apply Sort
              </button>
              
              <button mat-raised-button 
                      (click)="clearSort()"
                      [disabled]="sortCriteria.length === 0"
                      class="clear-all-sort-btn">
                <mat-icon>clear</mat-icon>
                Clear All
              </button>
            </div>

            <div class="sort-info" *ngIf="sortCriteria.length === 0">
              <mat-icon>info</mat-icon>
              <p>Click sort buttons to add columns to sorting</p>
            </div>
          </div>
        </div>
      </mat-tab>

      <!-- Freeze Columns Tab -->
      <mat-tab>
        <ng-template mat-tab-label>
          <mat-icon class="tab-icon">lock</mat-icon>
          Freeze Columns
        </ng-template>
        
        <div class="tab-content">
          <h4>Configure Freeze Columns</h4>
          <p class="freeze-info">Specify the number of columns to freeze from the left.</p>
          <mat-form-field appearance="outline">
            <mat-label>Number of Frozen Columns</mat-label>
            <input matInput type="number" min="0" max="3" 
                   [(ngModel)]="frozenColumnCount" 
                   (ngModelChange)="onFreezeColumnCountChange($event)"
                   placeholder="0">
          </mat-form-field>
          <p class="freeze-info" *ngIf="frozenColumnCount > 0">
            <mat-icon class="info-icon">info</mat-icon>
            Freezing {{ frozenColumnCount }} column(s) from the left
          </p>
        </div>
      </mat-tab>
    </mat-tab-group>
  </div>

<div class="table-wrapper" [class.fullscreen-wrapper]="isFullscreen">
  
  <div class="toolbar-actions">
    <!-- <p class="toolbarHeader">Handover Notes</p> -->
    <button mat-icon-button matTooltip="Column Options" (click)="toggleColumnMenu()">
      <mat-icon>view_column</mat-icon>
    </button>
    <button mat-icon-button  [matTooltip]="isFullscreen ? 'Exit Fullscreen' : 'Fullscreen'"  (click)="toggleFullscreen()">
      <mat-icon>{{ isFullscreen ? 'fullscreen_exit' : 'fullscreen' }}</mat-icon>
    </button>
  </div>
  
  <div class="resizable-table">
    <table mat-table [dataSource]="dataSource" matSort class="mat-elevation-z8 full-table">
      <ng-container
        *ngFor="let column of displayedColumns; let i = index"
        [matColumnDef]="column"
        [sticky]="i < frozenColumnCount"  >
        <th mat-header-cell *matHeaderCellDef [mat-sort-header]="column" appColumnResize #headerCell 
            [class.multi-sorted]="getColumnSortIndex(column) !== -1"
            [class.secondary-sorted]="getColumnSortIndex(column) > 0">

          <span class="header-content">
            <span class="column-name">{{ column }}</span>
            <span class="custom-sort-indicator" *ngIf="getColumnSortIndex(column) > 0">
              <mat-icon class="custom-sort-arrow">{{ getColumnSortDirection(column) === 'asc' ? 'arrow_upward' : 'arrow_downward' }}</mat-icon>
            </span>
          </span>
        </th>
        <td mat-cell *matCellDef="let element" 
            >{{ element[column] }}</td>
      </ng-container>

      <tr mat-header-row *matHeaderRowDef="displayedColumns; sticky: true"></tr>
      <tr mat-row *matRowDef="let row; columns: displayedColumns;"></tr>
    </table>
  </div>
</div>
