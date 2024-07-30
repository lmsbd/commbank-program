# Cover The `GetGoalsForUser` Route
- [ ] Arrange and Act using `Get` as a model
- [ ] Assert that:
    - [ ] `result` is not null
- [ ] For each `goal` in `result`, assert that:
    - [ ] It is assignable from `Goal`
    - [ ] It has the expected `UserId`


```cs
public class GoalControllerTests
{
    private readonly FakeCollections collections;

    public GoalControllerTests()
    {
        collections = new FakeCollections();
    }

    [Fact]
    public async Task GetForUser()
    {
        // Arrange
        var goals = collections.GetGoals();
        var users = collections.GetUsers();
        IGoalsService goalsService = new FakeGoalsService(goals, goals[0]);
        IUsersService usersService = new FakeUsersService(users, users[0]);
        GoalController controller = new GoalController(goalsService, usersService);

        // Setting up the HttpContext for the controller
        var httpContext = new Microsoft.AspNetCore.Http.DefaultHttpContext();
        controller.ControllerContext.HttpContext = httpContext;

        // Act
        var result = await controller.GetForUser(goals[0].UserId!);

        // Assert
        Assert.NotNull(result);
        
        var goalsResult = result as IEnumerable<Goal>;
        Assert.NotNull(goalsResult); // Ensure the result is of type IEnumerable<Goal>
        
        foreach (var goal in goalsResult!)
        {
            Assert.IsAssignableFrom<Goal>(goal); // Ensure each item is of type Goal
            Assert.Equal(goals[0].UserId, goal.UserId); // Ensure the UserId matches the expected value
        }
    }
}
        var index = 0;
        foreach (Goal goal in result!)
        {
            Assert.IsAssignableFrom<Goal>(goal);
            Assert.Equal(goals[0].UserId, goal.UserId);
            index++;
        }
    }
}

```
