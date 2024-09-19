# How to highlight the last selected row in Flutter DataTable (SfDataGrid)?.

In this article, we will show you how to highlight the last selected row in [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).

Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with all the necessary properties. Instead of setting the color in the [DataGridRowAdapter](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridRowAdapter-class.html), apply it directly to the container. In the [onSelectionChanged](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid/onSelectionChanged.html) callback, ensure you call [notifyListeners](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridSourceChangeNotifier/notifyListeners.html) from the DataGridSource when a new row is selected. By calling notifyListeners, all rows will be refreshed, which will trigger the buildRow method for each row. This approach allows a distinct selection color to be applied exclusively to the most recently selected row.

```dart
@override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Syncfusion Flutter DataGrid'),
      ),
      body: SfDataGrid(
        source: employeeDataSource,
        columnWidthMode: ColumnWidthMode.fill,
        controller: employeeDataSource.controller,
        selectionMode: SelectionMode.multiple,
        onSelectionChanged:
            (List<DataGridRow> newRows, List<DataGridRow> oldRows) {
          if (newRows.isNotEmpty) {
            employeeDataSource.updateDataGridSource();
          }
        },
        columns: getColumns(),
      ),
    );
  }

class EmployeeDataSource extends DataGridSource {

……

  DataGridController controller = DataGridController();

  @override
  DataGridRowAdapter? buildRow(DataGridRow row) {
    Color getRowBackgroundColor(DataGridRow row, {Color? color}) {
      if (controller.selectedRows.last == row) {
        return color ?? Colors.indigo;
      }
      return Colors.transparent;
    }

    return controller.selectedRows.isNotEmpty
        ? DataGridRowAdapter(
            cells: row.getCells().map<Widget>((dataGridCell) {
            return Container(
                color: getRowBackgroundColor(row),
                alignment: Alignment.center,
                child: Text(
                  dataGridCell.value.toString(),
                ));
          }).toList())
        : DataGridRowAdapter(
            cells: row.getCells().map<Widget>((dataGridCell) {
            return Container(
                color: Colors.transparent,
                alignment: Alignment.center,
                child: Text(
                  dataGridCell.value.toString(),
                ));
          }).toList());
  }

  void updateDataGridSource() {
    notifyListeners();
  }
}
```

You can download this example on [GitHub](https://github.com/SyncfusionExamples/How-to-highlight-the-last-selected-row-in-Flutter-DataTable).