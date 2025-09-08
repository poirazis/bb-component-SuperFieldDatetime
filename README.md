# Super Field - Datetime

A comprehensive date and time input component for Budibase applications with multiple formats, range selection, and flexible display options.

## 🚀 Features

### Date/Time Input

- **Datetime Fields**: Dedicated datetime field type with type safety
- **Date Range Selection**: Support for date range inputs
- **Time Inclusion**: Optional time component with hours/minutes
- **Multiple Formats**: Various date format options (US, European, ISO)
- **Default Values**: Pre-set date/time values

### Formatting & Display

- **Custom Templates**: Value formatting templates for display
- **Locale Support**: Format dates according to user locale
- **Visual Calendar**: Interactive calendar picker interface
- **Time Picker**: Intuitive time selection controls
- **Help Text**: Contextual guidance for date/time entry

### Validation & Control

- **Datetime Validation**: Date range, required field validation
- **Format Validation**: Ensures proper date/time format
- **Event Handling**: On change events with datetime context
- **State Management**: Disabled and readonly modes

### User Experience

- **Intuitive Interface**: Calendar popup with easy navigation
- **Keyboard Support**: Full keyboard date entry and navigation
- **Accessibility**: Screen reader support and ARIA labels
- **Mobile Friendly**: Touch-optimized date picker
- **Performance**: Efficient date rendering and selection

### Advanced Features

- **Button Integration**: Quick date selection buttons
- **Icon Support**: Visual indicators for date fields
- **Conditional Logic**: Dynamic behavior based on date values
- **Range Mode**: Select start and end date ranges

### Styling & Layout

- **Flexible Positioning**: Label placement options
- **Field Modes**: Form input or inline editing
- **Size Configuration**: Adjustable component width
- **Theme Integration**: Consistent with Budibase design

## 📝 Usage Instructions

### Basic Setup

1. Add the Super Field - Datetime component to your form
2. Bind to a datetime field in your data source
3. Choose between single datetime or date range mode
4. Configure date format and time display options

### Advanced Configuration

- **Format Selection**: Choose appropriate date format for your users
- **Time Display**: Enable time selection if needed
- **Validation**: Set date ranges and required field rules
- **Events**: Attach actions to date changes

### Common Use Cases

- **Appointment Scheduling**: Date and time selection for bookings
- **Event Planning**: Start and end date ranges
- **Data Filtering**: Date range filters for reports
- **Deadline Tracking**: Due date and time management
- **Historical Records**: Date-stamped data entry

## 🔧 Configuration Options

| Setting        | Type     | Description                      |
| -------------- | -------- | -------------------------------- |
| Field          | Datetime | Datetime field binding           |
| Label          | String   | Display label text               |
| Placeholder    | String   | Input guidance text              |
| Default Value  | Datetime | Pre-set date/time value          |
| Format Value   | Template | Display formatting template      |
| Help Text      | String   | Help/instruction text            |
| Validation     | Rules    | Date validation (range/required) |
| Control Type   | Select   | Datetime or Date Range mode      |
| Date Format    | Select   | Date display format              |
| Show Time      | Boolean  | Include time selection           |
| Icon           | Icon     | Visual indicator icon            |
| Disabled       | Boolean  | Disable date selection           |
| Read Only      | Boolean  | Read-only mode                   |
| Field Mode     | Select   | Form or inline input style       |
| Label Position | Select   | Label placement                  |
| Size           | Number   | Component width span             |

## 📋 Events

### On Change

Triggered when the date/time value changes.

**Context:**

- `value`: The selected datetime value
- `formattedValue`: The formatted display value
- `field`: The bound field information

## 🎨 Styling

The component supports Budibase's styling system:

- **Calendar Themes**: Consistent calendar appearance
- **Color Coding**: Visual date states and selections
- **Custom CSS**: Advanced styling customization
- **Responsive**: Mobile-optimized date picker

## 🔍 Best Practices

- Choose date formats familiar to your user base
- Use date ranges for filtering and reporting
- Enable time selection only when necessary
- Provide clear labels for date field purposes
- Consider timezone implications for datetime fields
- Test date picker on various devices and browsers
