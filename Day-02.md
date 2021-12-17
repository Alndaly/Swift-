# SwiftUI
> SwiftUI的keyboard需要增加一个Focus的状态，否则会遇到键盘出现后无法取消的现象

```swift
@FocusState private var amountIsFocused: Bool
```
```swift
.focused($amountIsFocused)
```
```swift
.toolbar {
    ToolbarItemGroup(placement: .keyboard) {  
		Spacer()//将Button挤到右侧
        Button("Done") {
            amountIsFocused = false
        }
    }
}
```
```swift
ToolbarItemGroup(placement: .keyboard) {
    Spacer()

    Button("Done") {
        amountIsFocused = false
    }
}
```
## 范例项目一 WeSplit
### 代码
```swift
import SwiftUI

struct ContentView: View {
	 @State private var checkAmount = 0.0
	 @State private var tipPercentage = 20
	 @State private var numberOfPeople = 0
	 @FocusState private var amountIsFocused:Bool
		 var totalPerPerson: Double {
		 let peopleCount = Double(numberOfPeople + 2)
		 let tipSelection = Double(tipPercentage)
		 let tipValue = checkAmount / 100 * tipSelection
		 let grandTotal = checkAmount + tipValue
		 let amountPerPerson = grandTotal / peopleCount
		 return amountPerPerson
	 }

	 let tipPercentages = [0,10,15,20,25]
	 var body: some View {
		 NavigationView{
			 Form{
				 Section{
				 TextField("Amount", value: $checkAmount, format: .currency(code: Locale.current.currencyCode ?? "USD"))
				 .keyboardType(.decimalPad)
				 .focused($amountIsFocused)
					 Picker("Number of people",selection: $numberOfPeople){
						 ForEach(2..<20){
						 Text("\($0) people")
						 }
					 }
				 }
				 Section {
					 Picker("Tip percentage", selection: $tipPercentage) {
						 ForEach(tipPercentages, id: \.**self**) {
						 Text($0, format: .percent)
						 }
					 }
				 .pickerStyle(.segmented)
				 } header: {
					Text("How much tip do you want to have?")
				 }
				 Section{
					 Text(totalPerPerson,format: .currency(code: Locale.current.currencyCode ?? "USD"))
				 }
			 }
			.navigationTitle("WeSplit")
			 .toolbar{
				 ToolbarItemGroup(placement:.keyboard){
					Spacer()
					 Button("Done"){
					 amountIsFocused = **false**
					 }
				 }
			 }
		 }
	 }
}

  

struct ContentView_Previews: PreviewProvider {
	 static var previews: some View {
		ContentView()
	 }
}
```
### 程序截图
![](IMG_9F015306471D-1.jpeg)