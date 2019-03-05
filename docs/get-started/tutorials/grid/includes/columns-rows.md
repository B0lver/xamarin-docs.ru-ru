# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Grid`](xref:Xamarin.Forms.Grid) для определения столбцов и строк и разместите содержимое в столбцах и строках:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <Label Text="Column 0, Row 0" />
        <Label Grid.Column="1"
               Text="Column 1, Row 0" />
        <Label Grid.Row="1"
               Text="Column 0, Row 1" />
        <Label Grid.Column="1"
               Grid.Row="1"
               Text="Column 1, Row 1" />
    </Grid>
    ```

    Этот код определяет столбцы и строки для [`Grid`](xref:Xamarin.Forms.Grid) и располагает экземпляры [`Label`](xref:Xamarin.Forms.Label) в конкретных столбцах и строках. Данные столбцов и строк хранятся в свойствах [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) и [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions), которые являются коллекциями из [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) и [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) соответственно. Ширина каждого `ColumnDefinition` задается свойством [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width), высота каждого `RowDefinition` — свойством [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height). Ниже приведены допустимые значения ширины и высоты.

    - [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto), который изменяет размер столбца или строки в соответствии с содержимым.
    - Пропорциональные значения, которые задают размер столбцов и строк в долях оставшегося пространства. Пропорциональные значения обозначаются в конце как `*`.
    - Абсолютные значения, которые задают фиксированный размер столбцов или строк.

    Таким образом, в приведенном выше коде каждый столбец имеет ширину в половину [`Grid`](xref:Xamarin.Forms.Grid), а каждая строка имеет высоту в 50 аппаратно-независимых единиц.

    Положение каждого [`Label`](xref:Xamarin.Forms.Label) в [`Grid`](xref:Xamarin.Forms.Grid) указывается с помощью вложенных свойств [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) и [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty), которые используют отсчитываемый от нуля индекс. Таким образом, первый столбец имеет номер 0, и первая строка имеет номер 0. У первого `Label` эти вложенные свойства отсутствуют, так как дочерние представления, не задающие их, автоматически будут отображаться в столбце 0 и строке 0.

    > [!NOTE]
    > Расстояние между столбцами и строками в [`Grid`](xref:Xamarin.Forms.Grid) можно задать с помощью свойств [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) и [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing). Дополнительные сведения см. в разделе [Интервалы](~/xamarin-forms/user-interface/layouts/grid.md#spacing) руководства [Сетка Xamarin.Forms](~/xamarin-forms/user-interface/layouts/grid.md).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана сетки с содержимым, разделенным по столбцам и строкам, в iOS и Android](../images/columns-rows.png "Сетка с содержимым, разделенным по столбцам и строкам")](../images/columns-rows-large.png#lightbox "Сетка с содержимым, разделенным по столбцам и строкам")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Grid`](xref:Xamarin.Forms.Grid) для определения столбцов и строк и разместите содержимое в столбцах и строках:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <Label Text="Column 0, Row 0" />
        <Label Grid.Column="1"
               Text="Column 1, Row 0" />
        <Label Grid.Row="1"
               Text="Column 0, Row 1" />
        <Label Grid.Column="1"
               Grid.Row="1"
               Text="Column 1, Row 1" />
    </Grid>
    ```

    Этот код определяет столбцы и строки для [`Grid`](xref:Xamarin.Forms.Grid) и располагает экземпляры [`Label`](xref:Xamarin.Forms.Label) в конкретных столбцах и строках. Данные столбцов и строк хранятся в свойствах [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) и [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions), которые являются коллекциями из [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) и [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) соответственно. Ширина каждого `ColumnDefinition` задается свойством [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width), высота каждого `RowDefinition` — свойством [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height). Ниже приведены допустимые значения ширины и высоты.

    - [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto), который изменяет размер столбца или строки в соответствии с содержимым.
    - Пропорциональные значения, которые задают размер столбцов и строк в долях оставшегося пространства. Пропорциональные значения обозначаются в конце как `*`.
    - Абсолютные значения, которые задают фиксированный размер столбцов или строк.

    Таким образом, в приведенном выше коде каждый столбец имеет ширину в половину [`Grid`](xref:Xamarin.Forms.Grid), а каждая строка имеет высоту в 50 аппаратно-независимых единиц.

    Положение каждого [`Label`](xref:Xamarin.Forms.Label) в [`Grid`](xref:Xamarin.Forms.Grid) указывается с помощью вложенных свойств [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) и [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty), которые используют отсчитываемый от нуля индекс. Таким образом, первый столбец имеет номер 0, и первая строка имеет номер 0. У первого `Label` эти вложенные свойства отсутствуют, так как дочерние представления, не задающие их, автоматически будут отображаться в столбце 0 и строке 0.

    > [!NOTE]
    > Расстояние между столбцами и строками в [`Grid`](xref:Xamarin.Forms.Grid) можно задать с помощью свойств [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) и [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing). Дополнительные сведения см. в разделе [Интервалы](~/xamarin-forms/user-interface/layouts/grid.md#spacing) руководства [Сетка Xamarin.Forms](~/xamarin-forms/user-interface/layouts/grid.md).

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана сетки с содержимым, разделенным по столбцам и строкам, в iOS и Android](../images/columns-rows.png "Сетка с содержимым, разделенным по столбцам и строкам")](../images/columns-rows-large.png#lightbox "Сетка с содержимым, разделенным по столбцам и строкам")
