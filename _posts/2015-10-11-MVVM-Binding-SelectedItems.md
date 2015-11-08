---
layout: post
date:   2015-10-11 13:00:00
title: "MVVM Binding SelectedItems"
---
Recently I had to use the SelectedItems property on a WPF ListView in a MVVM based application. This however turned out to be a bit of a problem because SelectedItems doesn't work when using databinding.

After some extensive google'ing I found a solution which involved overriding the ItemContainerStyle in order to add an IsSelected property to the model.

### SelectedItemsView.xaml

{% highlight xml %}
<!-- SelectedItemsView.xaml -->
<ListView ItemsSource="{Binding Path=Users}">
	<ListView.View>
		<GridView>
			<GridViewColumn Header="First Name" Width="120" DisplayMemberBinding="{Binding Path=FirstName}" />
			<GridViewColumn Header="Last Name" Width="120" DisplayMemberBinding="{Binding Path=LastName}" />
		</GridView>
	</ListView.View>
	<ListView.ItemContainerStyle>
		<Style TargetType="{x:Type ListViewItem}">
			<Setter Property="IsSelected" Value="{Binding Path=IsSelected, Mode=TwoWay}" />
		</Style>
	</ListView.ItemContainerStyle>
</ListView>
{% endhighlight %}

### User.cs

{% highlight csharp %}
// User.cs
public class User
{
	public string FirstName { get; set; }
	public string LastName { get; set; }
	public bool IsSelected { get; set; }
}
{% endhighlight %}

### SelectedItemsViewModel.cs

{% highlight csharp %}
// SelectedItemsViewModel.cs
public class SelectedItemsViewModel
{
	public ObservableCollection<User> Users { get; private set; }

	public SelectedItemsViewModel()
	{
		LoadUsers();
	}

	private void LoadUsers()
	{
		Users = new ObservableCollection<User>();
		Users.Add(new User { FirstName = "John", LastName = "Doe" });
		Users.Add(new User { FirstName = "Jane", LastName = "Doe" });
		Users.Add(new User { FirstName = "Joey", LastName = "Doe" });
		Users.Add(new User { FirstName = "Jackie", LastName = "Doe" });
	}
	
	public void PrintSelectedUsers()
	{
		var selectedUsers = Users.Where(u => u.IsSelected == true);
		
		foreach(var u in selectedUsers)
			Console.WriteLine("{0}, {1}", u.LastName, u.FirstName);
	}
}
{% endhighlight %}

