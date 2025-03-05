# 🚀 Nail Project Task note

**🌐 Tasks note 🌐**

### NAIL: Flow to validate Booking Service Slots:
  1. Validate Business is active => OK
    - Validate Business is empty employees => OK
    - Validate Business is empty services=> OK
    - Validate Business time (working time & holiday) => OK
    - If assigned to employee => Validate booking period
      + Validate Employee is active => OK
      + Validate Employee time (working time & holiday) => OK
      + Validate Employee active service => OK
      + Validate booking period not overlap with booked periods of this employee
    - Validate Business Service limit slot type